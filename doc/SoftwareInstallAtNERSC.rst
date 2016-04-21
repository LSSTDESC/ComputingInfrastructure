##################################
NERSC Software Installation for DESC
##################################

DMStack
==================================
Using the conda dev channel, we can install a relatively up to date version of lsst_apps and lsst_sims on Cori at NERSC.

How to use
----------------------------------
- Log onto Cori
- The installation can be found in: /project/projectdirs/lsst/lsstDM/Cori/conda-env
  most recent install is 2016-04-15
- source <PathToInstallation>/setupVanillaStack.sh

The above will update your environment to use the conda environment installed with DMstack.  To exit and reset your environment, at the 
command line do:
"source deactivate"

Installation Instructions
----------------------------------
.. code-block:: bash
   :name: conda-install-instructions
   
   module swap PrgEnv-intel PrgEnv-gnu
   module swap gcc gcc/4.9.3
   module load python/2.7-anaconda
   export CONDA_ENVS_PATH=$PWD/conda-env
   conda create --prefix /project/projectdirs/lsst/lsstDM/Cori/conda-env/<date>/LSST_STACK --channel http://eupsforge.net/conda/dev lsst-apps
   source activate /project/projectdirs/lsst/lsstDM/Cori/conda-env/2016-04-15/LSST_STACK
   conda install --channel http://eupsforge.net/conda/dev lsst-sims
   conda install nose
   conda install panda
   
afw expects to find libssl.so.10 and libcrypto.so.10

.. code-block:: bash
   :name: symlink-libs
      
    ln -s /project/projectdirs/lsst/lsstDM/Cori/conda-env/.pkgs/openssl-1.0.2g-0/lib/libcrypto.so.1.0.0 /project/projectdirs/lsst/lsstDM/Cori/conda-env/<date>/LSST_STACK/opt/lsst/afw/lib/libcrypto.so.10
    ln -s /project/projectdirs/lsst/lsstDM/Cori/conda-env/.pkgs/openssl-1.0.2g-0/lib/libssl.so.1.0.0 /project/projectdirs/lsst/lsstDM/Cori/conda-env/<date>/LSST_STACK/opt/lsst/afw/lib/libssl.so.10
