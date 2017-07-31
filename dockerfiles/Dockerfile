FROM opensuse:42.1

RUN zypper install -y bash \
        bison \
        bzip2 \
        cmake \
        curl \
        flex \
        gcc-c++ \
        gettext \
        git \
        libzip2 \
        glib2-devel \
        pthread-stubs-devel \
        readline-devel \
        libopenssl-devel \
        libSM6 \
        make \
        python-numpy \
        ncurses-devel \
        java-1_8_0-openjdk-headless \
        openssl \
        patch \
        python-devel \
        python-setuptools \
        zlib \
        zlib-devel \
        tar \
        libXext6 \
        libbz2-devel \
        libXrender1 \
        gcc48-fortran \
        which

ARG LSST_STACK_DIR=/lsst/stack
ARG LSST_USER=desc
ARG LSST_GROUP=desc
ARG LSST_WORKSPACE=/desc

ARG EUPS_HOME=$LSST_STACK_DIR/.eups
ARG EUPS_PRODUCT=lsst_distrib
ARG EUPS_TAG=v13_0
ARG INSTALL_MINICONDA=Y

RUN mkdir -p $LSST_STACK_DIR
RUN mkdir -p $EUPS_HOME
RUN mkdir -p $LSST_WORKSPACE

WORKDIR $LSST_STACK_DIR

RUN echo "Environment: \n" && env | sort

# Explicitly setup user and user groups
RUN groupadd $LSST_GROUP
RUN useradd -m -g $LSST_USER $LSST_GROUP
RUN usermod -s /bin/bash $LSST_USER

# Install stack
# If $EUPS_TAG exists, prepend with a "-t"
RUN echo "Executing: eups distrib install $EUPS_TAG_OPT $EUPS_PRODUCT $EUPS_PRODUCT_VERSION" && \
    curl -OL https://raw.githubusercontent.com/lsst/lsst/13.0/scripts/newinstall.sh && \
    echo $INSTALL_MINICONDA | bash newinstall.sh && \
    /bin/bash -c 'source $LSST_STACK_DIR/loadLSST.bash; \
                  eups list; \
                  eups distrib install ${EUPS_TAG:+"-t"}  $EUPS_TAG $EUPS_PRODUCT --nolocks;\
                  echo 'hooks.config.site.lockDirectoryBase = None' >> site/startup.py;'

# Make sure $LSST_STACK_DIR and $LSST_WORKSPACE have proper permissions
# Fixing $LSST_STACK_DIR takes a bit of time.
RUN chown -R $LSST_USER:$LSST_GROUP $LSST_WORKSPACE
