Opsim4 with singularity
-----------------------

Base instructions
~~~~~~~~~~~~~~~~~

These instructions are derived the ones in Owen Boberg's ```opsim4`` dockerhub <https://hub.docker.com/r/oboberg/opsim4/>`_.

Prepare environment variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update these as appropriate. For available tags, see `this page <https://hub.docker.com/r/oboberg/opsim4/tags/>`_.

.. code:: sh

    MYBASEDIR=/data/des70.a/data/neilsen
    OPSIMTAG=081217

Download sky brightess data
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Make a directory for sky brightness data, and run the script to
retrieve it. It downloads roughly 64G, and so takes a while.

.. code:: sh

    cd ${MYBASEDIR}
    git clone https://github.com/lsst/sims_skybrightness_pre
    cd sims_skybrightness_pre/data
    ./data_down.sh

Import the opsim4 image into singularity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sh

    mkdir ${MYBASEDIR}/singularity
    cd ${MYBASEDIR}/singularity
    singularity pull docker://oboberg/opsim4:${OPSIMTAG}

Copy the opsim home out of the singularity image
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Unlike when you start a container in ``docker``, when you start a
singularity container, you retain your own username, uid, group, and
gid. The files and directories you need to read and write in the
image, however, and owned by user ``opsim``, so if you run the image in
``singularity``, you won't have the needed permissions. So, copy this
directory out of the container. This way, when you start the
container, you can mount your own copy (owned by you) on
``/home/opsim``, and have the needed permissions.

.. code:: sh

    cd ${MYBASEDIR}/singularity
    singularity export -f opsim4-${OPSIMTAG}.tar opsim4-${OPSIMTAG}.img
    mkdir opsim4-${OPSIMTAG}
    cd opsim4-${OPSIMTAG}
    tar --same-permissions --no-same-owner --extract \
        --file=../opsim4-${OPSIMTAG}.tar ./home/opsim
    cd ..

Make other needed directories
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sh

    mkdir -p ${MYBASEDIR}/opsim4_data/run_local
    mkdir -p ${MYBASEDIR}/opsim4_data/other-configs
    mkdir -p ${MYBASEDIR}/opsim4_data/.config

Start the singularity container
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sh

    env -i OPSIM_HOSTNAME=$(echo $HOSTNAME | cut -f1 -d.) \
           DISPLAY=$DISPLAY \
           singularity shell \
     --bind ${MYBASEDIR}/singularity/opsim4-${OPSIMTAG}/home/opsim:/home/opsim \
     --bind ${MYBASEDIR}/opsim4_data/run_local:/home/opsim/run_local \
     --bind ${MYBASEDIR}/opsim4_data/other-configs:/home/opsim/other-configs \
     --bind ${MYBASEDIR}/opsim4_data/.config:/home/opsim/.config \
     --bind ${MYBASEDIR}/sims_skybrightness_pre/data:/home/opsim/sky_brightness_data \
     --bind ${MYBASEDIR}/sims_skybrightness_pre/data:/home/opsim/repos/sims_skybrightness_pre/data \
     ${MYBASEDIR}/singularity/opsim4-${OPSIMTAG}.img

Build the LSST products that need building
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This need only be done once per container:

.. code:: sh

    source startup.sh
    cd repos/sims_skybrightness_pre/
    setup sims_skybrightness_pre git
    scons
    cd ~/repos/ts_astrosky_model
    setup ts_astrosky_model git
    scons

Prepare the output directory and database
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you do not already have a configured output directory
in ``${MYBASEDIR}/opsim4_data/run_local`` (again within the container):

.. code:: sh

    cd ~
    setup ts_scheduler
    setup sims_ocs
    mkdir ~/run_local/output
    manage_db --save-dir=$HOME/run_local/output

Start an ``opsim`` simulation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once more from within the container:

.. code:: sh

    cd ~
    source startup.sh ;# if you have not done this already
    opsim4 --frac-duration=0.003 \
           --scheduler-timeout=600 \
           -c "my description of this simulation" \
           -v

The ``--frac-duration`` parameter gives the duration of the simulation
to do in units of years.
