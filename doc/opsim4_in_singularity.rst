Opsim4 with singularity
-----------------------

Base instructions
~~~~~~~~~~~~~~~~~

These instructions are derived the ones in Owen Boberg's ```opsim4`` dockerhub <https://hub.docker.com/r/oboberg/opsim4/>`_.

While logged into the host where you will be running it, prepare the environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For available ``opsim4`` tags, see `this page <https://hub.docker.com/r/oboberg/opsim4/tags/>`_.

.. code:: sh

    MYBASEDIR=/data/des70.a/data/neilsen

    mkdir -P ${MYBASEDIR}/singularity
    mkdir -P ${MYBASEDIR}/opsim4_data/run_local
    mkdir -P ${MYBASEDIR}/opsim4_data/other-configs
    mkdir -P ${MYBASEDIR}/opsim4_data/.config

Convert the opsim4 docker image into a singularity tarball
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Find a host with a privileged singularity installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Although singularity containers can be run in "unprivileged"
installations, ``docker`` images can only be imported using a
"privileged" installation. 

If you already have access to such a host, skip the next
section. If you have a ``fermicloud`` account, you can follow the next
set of instructions to create one.

Create a fermicloud VM and install singularity on it
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you already have access to a host with a privileged ``singularity``
installation, skip these instructions. If you do not have a
``fermicloud`` account, you may need to find some other way to obtain a
linux host (virtual or otherwise) on which you have root access.

Begin by logging into the ``fermicloud`` interface node. For example:

.. code:: sh

    ssh neilsen@fermicloudui.fnal.gov

On ``fermicloudui.fnal.gov``, instantiate an SLF7 VM:

.. code:: sh

    onetemplate instantiate "SLF7Vanilla_DynamicIP"  --name "$USER test VM" 
    watch -n 10 "onevm list ; one_check-pingVMs.sh"

Wait for ``one_check-pingVMs.sh`` to give results indicating the new VM
is pingable. It will look something like this:

::

    Every 10.0s: onevm list ; one_check-pingVMs.sh                                                                                                                                                                                                                                                                                                    Wed Jan  3 15:12:12 2018

        ID USER     GROUP    NAME            STAT UCPU    UMEM HOST             TIME
     42728 neilsen  users    neilsen test VM runn  103    1.9G fcl009	0d 00h03
    +OK - Pingable VMs (1/1):
        42728,neilsen,users,neilsen test VM,runn,103,1.9G,fcl009,  0d 00h03m    fermicloud309.fnal.gov.

It may take a few minutes. Use ^C to exit ``watch``.

Note the host name for your new VM (``fermicloud309.fnal.gov`` in the
above example).

You can log out of ``fermicloudui.fnal.gov`` now.

Then, connect to you new VM, for example:

.. code:: sh

    ssh root@fermicloud309.fnal.gov

On the VM, follow the instructions on
`http://singularity.lbl.gov/install-linux <http://singularity.lbl.gov/install-linux>`_ to install singularity:

.. code:: sh

    VERSION=2.3.2

    wget https://github.com/singularityware/singularity/releases/download/$VERSION/singularity-$VERSION.tar.gz
    tar xvf singularity-$VERSION.tar.gz
    cd singularity-$VERSION
    ./configure --prefix=/usr/local
    make
    make install

Import the ``opsim4`` image from ``dockerhub``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

On a host with a privileged ``singularity`` installed, import the
``opsim4`` docker image:

.. code:: sh

    OPSIMTAG=081217

    singularity pull docker://oboberg/opsim4:${OPSIMTAG}

Export the ``opsim4`` ``singularity`` image to a tar file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once ``singularity`` has pulled the ``opsim4`` image, you can export it to
a tar file and copy it to the host where you actually want to run
``opsim4`` (which may have an unprivileged ``singularity``):

.. code:: sh

    OPSIMTAG=081217
    MYBASEDIR=/data/des70.a/data/neilsen
    MYHOSTNAME=des70.fnal.gov
    MYUSERNAME=neilsen

    singularity export -f opsim4-${OPSIMTAG}.tar opsim4-${OPSIMTAG}.img
    scp opsim4-${OPSIMTAG}.tar ${MYUSERNAME}@${MYHOSTNAME}:${MYBASEDIR}/singularity/opsim4-${OPSIMTAG}.tar

Download sky brightess data
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Make a directory for sky brightness data, and run the script to
retrieve it. It downloads roughly 64G, and so takes a while.

.. code:: sh

    MYBASEDIR=/data/des70.a/data/neilsen

    cd ${MYBASEDIR}
    git clone https://github.com/lsst/sims_skybrightness_pre
    cd sims_skybrightness_pre/data
    ./data_down.sh

Untar the opsim4 image tar file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

While it is possible to run an image from the tar file directly, there
are a number of disadvantages to doing so. Instead, untar the image
into a directory.

.. code:: sh

    MYBASEDIR=/data/des70.a/data/neilsen
    OPSIMTAG=081217

    cd ${MYBASEDIR}/singularity
    mkdir opsim4-${OPSIMTAG}
    cd opsim4-${OPSIMTAG}
    tar -xf ../opsim4-${OPSIMTAG}.tar

Make mountpoints in the container for external directories
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sh

    MYBASEDIR=/data/des70.a/data/neilsen
    OPSIMTAG=081217

    cd ${MYBASEDIR}/singularity/opsim4-${OPSIMTAG}
    mkdir home/opsim/run_local
    mkdir home/opsim/other-configs
    mkdir home/opsim/sky_brightness_data
    mkdir hosthome

Start the singularity container
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sh

    MYBASEDIR=/data/des70.a/data/neilsen
    OPSIMTAG=081217
    OPSIM_HOSTNAME=$(hostname | cut -d. -f1)

    cd ${MYBASEDIR}/singularity/
    env -i OPSIM_HOSTNAME=${OPSIM_HOSTNAME} DISPLAY=${DISPLAY} \
     singularity shell \
      -H $HOME:/hosthome \
      --bind ${MYBASEDIR}/opsim4_data/run_local:/home/opsim/run_local \
      --bind ${MYBASEDIR}/opsim4_data/other-configs:/home/opsim/other-configs \
      --bind ${MYBASEDIR}/opsim4_data/.config:/home/opsim/.config \
      --bind ${MYBASEDIR}/sims_skybrightness_pre/data:/home/opsim/sky_brightness_data \
      --bind ${MYBASEDIR}/sims_skybrightness_pre/data:/home/opsim/repos/sims_skybrightness_pre/data \
      ${MYBASEDIR}/singularity/opsim4-${OPSIMTAG}

Build the LSST products that need building
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This need only be done once per container.

Within your newly started container:

.. code:: sh

    source /home/opsim/startup.sh
    cd /home/opsim/repos/sims_skybrightness_pre/
    setup sims_skybrightness_pre git
    scons
    cd /home/opsim/repos/ts_astrosky_model
    setup ts_astrosky_model git
    scons

Prepare the output directory and database
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Still within your container, if you do not already have a configured
output directory in ``${MYBASEDIR}/opsim4_data/run_local``:

.. code:: sh

    source /home/opsim/startup.sh ;# if you have not done this already
    setup ts_scheduler
    setup sims_ocs
    mkdir /home/opsim/run_local/output
    manage_db --save-dir=$HOME/run_local/output

Start an ``opsim`` simulation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once more from within the container:

.. code:: sh

    source /home/opsim/startup.sh ;# if you have not done this already
    setup ts_scheduler
    setup sims_ocs
    cd /home/opsim/run_local/output
    opsim4 --frac-duration=0.003 \
           --scheduler-timeout=600 \
           -c "1st test in my new installation" \
           -v

The ``--frac-duration`` parameter gives the duration of the simulation
to do in units of years.
