======================
Running OpSim at NERSC
======================




Introduction
------------

This document contains notes on converting the ``OpSim4`` ``docker`` image
to ``shifter``, and running it at NERSC. **These notes do not succeed;**
``OpSim4`` will not actually run. However, these notes do provide
information on overcoming some initial hurdles, and should be
generally useful on converting ``docker`` images to ``shifter`` and
running them at NERSC.

NERSC documentation for using ``docker`` images
-----------------------------------------------

The primary documentation for using ``docker`` images at NERSC can be
found here:

- `http://www.nersc.gov/users/software/using-shifter-and-docker/using-shifter-at-nersc/ <http://www.nersc.gov/users/software/using-shifter-and-docker/using-shifter-at-nersc/>`_

LSST project provided instructions for running ``OpSim4``
---------------------------------------------------------

The LSST project provides instructions for installing and running
``OpSim4``:

- `https://hub.docker.com/r/oboberg/opsim4/ <https://hub.docker.com/r/oboberg/opsim4/>`_

- `https://github.com/lsst-sims/sims_ocs/blob/master/doc/usage.rst <https://github.com/lsst-sims/sims_ocs/blob/master/doc/usage.rst>`_

- `https://lsst-sims.github.io/sims_ocs/installation.html <https://lsst-sims.github.io/sims_ocs/installation.html>`_

The first of these links provides the ``docker`` image itself, and the
primary instuctions on which these are based. However, these
instructions do not run exactly as provided, because disks provided in
``shifter`` images can only be mounted read-only.

Logging in to NERSC and preparing to run
----------------------------------------

Running ``opsim4`` (even from a ``docker`` container) requres checking
code out of github, which uses ``ssh`` authentication. Rather than
putting my private ``ssh`` key on ``cori.nersc.gov`` or another NERSC
host, I prefer to generate the authentication on my laptop and forward
it to ``cori``. For example:

::

    mac-124574:~ neilsen$ ssh-add
    Enter passphrase for /Users/neilsen/.ssh/id_rsa: 
    Identity added: /Users/neilsen/.ssh/id_rsa (/Users/neilsen/.ssh/id_rsa)
    Identity added: /Users/neilsen/.ssh/identity (neilsen@fnal.gov)
    mac-124574:~ neilsen$ ssh -A -o forwardX11=yes cori.nersc.gov

After the banner and announcements from NERSC, check to see the
environment is okay, with your desired public key represented by your
agent at the NERSC host, for example

::

    neilsen@cori08:~> ssh-add -L
    ssh-rsa <MY PUBLIC KEY> /Users/neilsen/.ssh/id_rsa

Finally, start a ``GNU screen`` session in case I get disconnected:

::

    neilsen@cori08:~> screen

Get sky brightness model data
-----------------------------

These instructions put the data in the ``SCRATCH`` area, which is
temporary.

On cori (not in the ``shifter`` container):

.. code:: sh

    cd $SCRATCH
    git clone https://github.com/lsst/sims_skybrightness_pre
    cd sims_skybrightness_pre
    cd data
    ./data_down.sh -o

The ``data_down.sh`` script uses ``rsync`` to download about 64G of
``numpy`` data files.

Convert the ``opsim4`` ``docker`` image into ``shifter``
--------------------------------------------------------

For a given tag, I think this needs only to be done once, for
everybody. As I have already done it with the image in this example,
it should not need to be done again unless you want to use a different
image or tag.

The list of tags can be found here:
`https://hub.docker.com/r/oboberg/opsim4/tags/ <https://hub.docker.com/r/oboberg/opsim4/tags/>`_

The tag I use here is 061117. The command to grab the tag from
dockerhub and convert it to a ``shifter`` image available at NERSC is:

.. code:: sh

    shifterimg -v pull docker:oboberg/opsim4:061117

This command took nearly 2 hours to finish.

Start the ``opsim`` shifter image
---------------------------------

Allocate an interactive node
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This command requests a node for interactive use, and makes the
``opsim4`` image available for starting with ``shifter``:

.. code:: sh

    salloc -N 1 --qos=interactive \
      --mem=8192 --mincpus=3 \
      --image=docker:oboberg/opsim4:061117 \
      -t 03:30:00 -L SCRATCH -C haswell

This command requests the node for only 3.5 hours. (NERSC imposes
fairly short upper limit on interactive nodes.)

The result of running this ``salloc`` command on ``cori`` is a prompt on a
newly allocated node. 

Start the shifter
~~~~~~~~~~~~~~~~~

The ``salloc`` command above allocates the node and provides a prompt on
it, but it doesn't actually start ``shifter``. To do that:

.. code:: sh

    MYNERSCUSER=${USER} shifter --volume=$SCRATCH:/mnt

The ``--volume`` parameter provides a way for the container to write to
the NERSC scratch disk.

Copy the home area from the container to ``$SCRATCH``
-----------------------------------------------------

Once the container is started, and the ``SCRATCH`` disk mounted within
it, then the ``home`` area in the container can be copied into the
``SCRATCH`` disk. For example, from within the ``shifter`` container:

.. code:: sh

    cd /home
    MYNERSCHOME=$(readlink -f $(ls -d /global/u?/?/${MYNERSCUSER} | head -1))
    mkdir -p ${MYNERSCHOME}/shifter_disks/opsim4/061117/home
    rsync -rlpt opsim ${MYNERSCHOME}/shifter_disks/opsim4/061117/home

I put the home are in the container in my NERSC home disk rather than
the NERSC scratch disk, because, although the scratech disk is well
optimized for reading and writing to long files, there can be a long
latency in opening files and getting file attributes, and the home
area is better optimized for smaller files: things like ``eups`` ``setup``
commands and loading ``python`` modules are **much** faster when they read
their data from the home area.

Now, exit the container (log out), so that you can restart it with the
writable home area.

Of course, replace ``neilsen`` with your own username.

Restart the ``shifter`` image, using your new copy of the home area as the home area
------------------------------------------------------------------------------------

Start a new ``shifter`` container, this time mounting the copy of the
home area as the home area. At the same time, have shifter give you
the shell created by the ``startup.sh`` script, which includes the setup
necessary for ``eups``.

.. code:: sh

    env -i OPSIM_HOSTNAME=ehntesthost1 \
      shifter --image=docker:oboberg/opsim4:061117 \
        --volume=$SCRATCH:/mnt \
        --volume=$HOME/shifter_disks/opsim4/061117/home/opsim:/home/opsim \
        -- /home/opsim/startup.sh

Now, you should be in a container in which the home area is writable (and
conveniently accessable outside of ``shifter``).

Test that you can write a file in the ``opsim`` home directory:

.. code:: sh

    touch ~opsim/foo

If this doesn't complain, you're good to go.

Make needed directories and links
---------------------------------

The base instructions provided with the ``docker`` image instruct us to
make several symbolic links from the ``opsim`` home directory in the
container. Because we are using a copy of the home directory, I try to
achieve the same effect by a slightly different method. From within
the shell prompt in the container:

.. code:: sh

    cd /home/opsim
    mkdir -p /mnt/opsim4/run_local
    ln -s /mnt/opsim4/run_local /home/opsim/run_local
    mkdir /mnt/opsim4/other-configs
    ln -s /mnt/opsim4/other-configs /home/opsim/other-configs
    ln -s /mnt/sims_skybrightness_pre/data /home/opsim/sky_brightness_data 
    mv /home/opsim/repos/sims_skybrightness_pre/data /home/opsim/repos/sims_skybrightness_pre/data_orig
    ln -s /mnt/sims_skybrightness_pre/data /home/opsim/repos/sims_skybrightness_pre/data
    mkdir /home/opsim/.config

Building and testing ``OpSim4``
-------------------------------

This set of incantations come directly from the ``docker`` instructions:

.. code:: sh

    cd
    cd repos/sims_skybrightness_pre/
    setup sims_skybrightness_pre git
    scons
    cd ~/repos/ts_astrosky_model
    setup ts_astrosky_model git
    scons
    cd ~
    setup ts_scheduler
    setup sims_ocs
    mkdir ~/run_local/output
    manage_db --save-dir=$HOME/run_local/output

Fiddle timeouts to make it wait longer
--------------------------------------

Initial attempts to run ``OpSim4`` resulted in many timeouts. The
timeout durations are hardcoded in the ``python`` source, so I edit them
directly. (I note that the contents of ``/home/opsim/repos`` include
``git`` repos, so we can revert to the originals using ``git checkout``.)

.. code:: sh

    sed -i 's/tf - lastconfigtime > 10/tf - lastconfigtime > 300/g' \
     /home/opsim/repos/ts_scheduler/python/lsst/ts/scheduler/main.py
    sed -i 's/tf - lastconfigtime > 20/tf - lastconfigtime > 300/g' \
     /home/opsim/repos/ts_scheduler/python/lsst/ts/scheduler/main.py
    sed -i 's/self.socs_timeout = 180.0/self.socs_timeout = 300.0/g' \
     /home/opsim/repos/sims_ocs/python/lsst/sims/ocs/kernel/simulator.py

Create the directory into which the scheduler expects to put its logs
---------------------------------------------------------------------

This directory name to be hard coded (relative to the scheduler python package):

.. code:: sh

    mkdir /home/opsim/repos/ts_scheduler/python/lsst/ts/log

Run a test sim
--------------

.. code:: sh

    cd ~/run_local
    setup sims_skybrightness_pre git
    setup ts_astrosky_model git
    setup ts_scheduler
    setup sims_ocs
    opsim4 --frac-duration=0.003 -c "test one day simaultion" -v --scheduler-timeout 600

The simulation does not actually run correctly, resulting in this in
the log file:

::

    Scheduler: run: scheduler config timeout
    Number of targets received: 0
    Number of observations made: 0
    Number of targets missed: 0
    Ending simulation
    An exception was thrown in SOCS!
    Traceback (most recent call last):
      File "/home/opsim/repos/sims_ocs/scripts/opsim4", line 119, in main
        sim.run()
      File "/home/opsim/repos/sims_ocs/python/lsst/sims/ocs/kernel/simulator.py", line 231, in run
        self.get_target_from_scheduler()
      File "/home/opsim/repos/sims_ocs/python/lsst/sims/ocs/kernel/simulator.py", line 174, in get_target_from_scheduler
        raise SchedulerTimeoutError("The Scheduler is not serving targets!")
    SchedulerTimeoutError: The Scheduler is not serving targets!
    Waiting for Scheduler to finish.
    Stopping programs.

This seems to happen as a result of line 118 of 
``/home/opsim//repos/ts_scheduler/python/lsst/ts/scheduler/main.py``:

.. code:: python

    scode = self.sal.getNextSample_schedulerConfig(self.topic_schedulerConfig)

return of -100. ``self.sal`` is an instance of a class imported from the
``SALPY_scheduler`` module defined in
``/mnt/opsim4/run_local/SALPY_scheduler.so``.

Recipe for running opsim on an already configured ``shifter`` image
-------------------------------------------------------------------

Ask for a node on which to run
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sh

    salloc -N 1 --qos=interactive \
      --mem=8192 --mincpus=3 \
      --image=docker:oboberg/opsim4:061117 \
      -t 03:45:00 -L SCRATCH -C haswell

This should put you in a shell on the allocated node.

Start the ``opsim`` ``shifter`` container
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

On the newly allocated node:

.. code:: sh

    env -i OPSIM_HOSTNAME=${USER}opsimhost \
      shifter --image=docker:oboberg/opsim4:061117 \
        --volume=$SCRATCH:/mnt \
        --volume=$HOME/shifter_disks/opsim4/061117/home/opsim:/home/opsim \
        -- /home/opsim/startup.sh

This should put you in a shell in the container.

Prepare the environment in the container
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In the container:

.. code:: sh

    cd ~/run_local
    setup sims_skybrightness_pre git
    setup ts_astrosky_model git
    setup ts_scheduler
    setup sims_ocs

Start ``opsim``
~~~~~~~~~~~~~~~

In the same shell configured above:

.. code:: sh

    opsim4 --frac-duration=0.003 -c "test one day simaultion" -v --scheduler-timeout 600

It still fails as before, though.
