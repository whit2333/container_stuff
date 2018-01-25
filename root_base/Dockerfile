# ROOT base
#
# A container for the latest root
#
FROM ubuntu:artful
LABEL maintainer "Whitney Armstrong <warmstrong@anl.gov>"
#
RUN apt-get update  \
      && apt-get update \
      && apt-get upgrade -y \
      && apt-get install -y \
      git dpkg-dev cmake g++ gcc binutils libx11-dev libxpm-dev \
      libxft-dev libxext-dev \
      && apt-get install -y \
      gfortran libssl-dev libpcre3-dev \
      xlibmesa-glu-dev libglew1.5-dev libftgl-dev \
      libmysqlclient-dev libfftw3-dev libcfitsio-dev \
      graphviz-dev libavahi-compat-libdnssd-dev \
      libldap2-dev python-dev libxml2-dev libkrb5-dev \
      libgsl0-dev libqt4-dev  software-properties-common apt-utils \
      && apt-get update \
      && add-apt-repository -y ppa:ubuntu-toolchain-r/test \
      && apt-get update \
      && apt-get install -y \
      make cmake gcc g++ curl wget \
      libxmu-dev libxmuu-dev git dpkg-dev  binutils language-pack-en \
      libx11-dev libxpm-dev libxft-dev libxext-dev gfortran libxi-dev\
      libssl-dev libpcre3-dev  xlibmesa-glu-dev libglew1.5-dev libxi6\
      libfftw3-dev graphviz-dev \
      libavahi-compat-libdnssd-dev  libldap2-dev python-dev \
      libxml2-dev libkrb5-dev libgsl0-dev libqt4-dev nano vim \
      libxerces-c-dev libboost-all-dev \
      && apt-get update \
      && apt-get install -y gcc-7 g++-7 gfortran-7 \
      && gcc --version \
      && update-alternatives --remove-all cc  \
      && update-alternatives --install /usr/bin/cc cc /usr/bin/gcc-7 30 \
      && update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 30 \
      && update-alternatives --set cc /usr/bin/gcc-7 \
      && update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++-7 30 \
      && update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 30 \
      && update-alternatives --set c++ /usr/bin/g++-7 \
      && update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-7 30 \
      && update-alternatives --set gfortran /usr/bin/gfortran-7 \
      && update-alternatives --config gcc \
      && update-alternatives --config g++ \
      && update-alternatives --config gfortran \
      && apt-get clean \
      && ls -lrth /usr/bin/gcc* \
      && gfortran --version \
      && gcc --version      \
      && cc --version       \
      && g++ --version      \
      && echo "  TEST  "     \
      && cd /tmp \
      && wget https://cmake.org/files/v3.10/cmake-3.10.1.tar.gz  \
      && tar -zxvf cmake-3.10.1.tar.gz \
      && cd cmake-3.10.1 && mkdir build && cd build \
      && cmake ../. && make -j10 install

RUN cd /tmp \
      && wget http://bitbucket.org/eigen/eigen/get/3.3.4.tar.bz2 \
      && tar -xvf 3.3.4.tar.bz2 \
      && cd eigen-* \
      && mkdir build && cd build \
      && cmake ../. \
      && make && make install \
      && apt-get install libeigen3-dev 


RUN cd /tmp \
      && git clone --depth=1 https://gitlab.cern.ch/CLHEP/CLHEP.git \
      && mkdir -p builds/clhep_build \
      && cd  builds/clhep_build \
      && cmake /tmp/CLHEP/.  \
      && make -j38 install \
      && cd /tmp && rm -rf /tmp/CLHEP && rm -rf /tmp/builds/clhep_build

# Build root from the repo master
RUN cd /tmp \
      && pwd \
      && git clone --depth=1 http://root.cern.ch/git/root.git root_master \
      && mkdir -p builds/root_build \
      && cd builds/root_build \
      && cmake ../../root_master/. -Dcxx14:BOOL=ON -Dgdml:BOOL=ON -Dmathmore:BOOL=ON -Dminuit2:BOOL=ON \
      && make -j38 install \
      && cd /tmp && rm -rf /tmp/root_master && rm -rf /tmp/builds/root_build 

RUN useradd -ms /bin/bash -d /opt/bubble_user bubble_user
USER bubble_user
#WORKDIR /opt/bubble_user

##CMD ["-c" ]
#ENTRYPOINT ["/bin/bash"]

CMD ["-b" ]
ENTRYPOINT ["root","-l"]

#RUN  cd /tmp \
#      && rm -r builds/clhep_build
#
#RUN ls -lrth /usr/local/lib/libRIO*
#RUN cd /tmp && pwd && ls -lrth
#RUN cd /tmp \
#      && wget http://geant4.web.cern.ch/geant4/support/source/geant4.10.04.tar.gz \
#      && tar -zxf geant4.10.04.tar.gz && rm geant4.10.04.tar.gz \
#      && mkdir -p builds/geant4_build \
#      && cd builds/geant4_build \
#      && cmake \
#        -DGEANT4_USE_GDML:BOOL=ON \
#        -DGEANT4_USE_QT:BOOL=ON \
#        -DGEANT4_USE_SYSTEM_CLHEP:BOOL=ON \
#        -DGEANT4_INSTALL_DATA=ON \
#        -DGEANT4_BUILD_CXXSTD:STRING=14 \
#        ../../geant4.10.04 \
#      && make -j37 install \
#      && cd /tmp && rm -rf /tmp/geant4.10.03.p02 && rm -rf /tmp/builds/geant4_build
#RUN cd /tmp \
#      && rm -r builds/geant4_build
#RUN cd /tmp && pwd && ls -lrth
#RUN source /usr/local/bin/thisroot.sh  \
#      && source /usr/local/bin/geant4.sh \
#RUN ["/bin/bash", "-c", "source /usr/local/bin/thisroot.sh \
#      && cd /tmp \
#      && ls -lrth \
#      && ls -lrth /usr/local/bin/* \
#      && git clone --depth=1 https://github.com/AIDASoft/DD4hep.git \
#      && mkdir -p builds/dd4hep_build \
#      && cd  builds/dd4hep_build \
#      && cmake ../../DD4hep/. -DCMAKE_INSTALL_PREFIX=/usr/local \
#      -DDD4HEP_USE_CXX14:BOOL=ON -DDD4HEP_USE_GEANT4:BOOL=ON  -DCMAKE_CXX_STANDARD:STRING=14 \
#      && make VERBOSE=1 -j38 install \
#      && cd /tmp && rm -rf /tmp/DD4hep && rm -rf /tmp/builds/dd4hep_build"]

#RUN cd /tmp  \
#      && git clone --depth=1 https://github.com/whit2333/CADMesh.git \
#      && cd CADMesh && mkdir -p build \
#      && cd build \
#      && cmake ../. \
#      && make -j34 install \
#      && cd /tmp && rm -rf /tmp/CADMesh 
#### Build root from the repo master
##RUN cd /tmp \
##      && pwd \
##      && git clone --depth=1 http://root.cern.ch/git/root.git root_master \
##      && mkdir -p builds/root_build \
##      && cd builds/root_build \
##      && cmake ../../root_master/. -Dcxx14:BOOL=ON -Dgdml:BOOL=ON -Dmathmore:BOOL=ON -Dminuit2:BOOL=ON \
##      && make -j38 install \
##      && cd /tmp && rm -rf /tmp/root_master && rm -rf /tmp/builds/root_build 
## 
##RUN cd /opt \
##      && mkdir -p sane08  \
##      && cd sane08  \
##      && git clone --depth=1 https://eicweb.phy.anl.gov/whit/insane.git insane \
##      && git clone --depth=1 https://eicweb.phy.anl.gov/whit/BETAG4
##
##RUN cd /tmp \
##      && mkdir -p build/insane \
##      && cd build/insane \
##      && cmake /opt/sane08/insane/. \
##      && make -j38 \
##      && make install \
##      && cd /tmp \
##      && rm -rf build
##RUN cd /tmp \
##      && mkdir -p build/betag4 \
##      && cd build/betag4 \
##      && cmake /opt/sane08/BETAG4/. \
##      && make -j38 \
##      && make install \
##      && cd /tmp \
##      && rm -rf build
##
###RUN cd /usr/local/lib \
###      && wget https://hallcweb.jlab.org/experiments/sane/whit/cernlib_2006.tar.gz -O - | tar -xvz 
##
##SHELL ["/bin/bash", "-c"]
##
##RUN useradd -ms /bin/bash -d /opt/saneuser saneuser \
##      && apt-get install  -y cernlib-* 
##
##ADD  saneuser_home_dir.tar /root/.
##ADD  saneuser_home_dir.tar /opt/saneuser/.
##
##RUN mkdir -p /opt/sane08 \
##      && cd /opt/sane08 \
##      && ls -lrth \
##      && git clone --depth=1 https://eicweb.phy.anl.gov/whit/Analyzer_new \
##      && git clone --depth=1 https://eicweb.phy.anl.gov/whit/sane_replay \ 
##      && cd /opt/sane08/Analyzer_new && make
##
##RUN mkdir -p /opt/saneuser/bin \
##      && cd /opt/saneuser/bin/ \
##      && wget https://hallcweb.jlab.org/experiments/sane/whit/sane08_farm_scripts.tar.gz -O - |  tar -xzv \
##      && chown -R saneuser:saneuser /opt/saneuser \
##      && chown -R saneuser:saneuser /opt/sane08 \
##      && cd /opt/sane08  \
##      && ln -s /opt/sane08/insane InSANE \
##      && cd /opt/saneuser \
##      && ln -s /opt/sane08  \
##      && ls -lrth
##
##
##USER saneuser
##WORKDIR /opt/saneuser

