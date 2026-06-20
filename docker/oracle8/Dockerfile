FROM oraclelinux:8

COPY epel.repo /etc/yum.repos.d

RUN yum update -y && \
    yum install -y ragel make wget python38 rpm-build git \
    qt5-qtbase-devel qt5-linguist qt5-qttranslations \
    openssl-devel boost-devel zlib-devel libcurl-devel \
    gcc gcc-c++ gcc-toolset-12 gcc-toolset-12-gcc-c++ \
    zip unzip curl perl perl-IPC-Cmd autoconf automake libtool \
    elfutils-devel && \
    yum clean all

RUN wget https://github.com/Kitware/CMake/releases/download/v3.30.5/cmake-3.30.5-linux-x86_64.sh && \
    chmod 755 cmake-3.30.5-linux-x86_64.sh && \
    ./cmake-3.30.5-linux-x86_64.sh --prefix=/opt/ --exclude-subdir --skip-license && \
    rm cmake-3.30.5-linux-x86_64.sh

RUN wget https://github.com/ninja-build/ninja/releases/download/v1.10.2/ninja-linux.zip && \
    unzip ninja-linux.zip && chmod 755 ninja && mv ninja /opt/bin && rm ninja-linux.zip

# Use gcc-toolset-12 so vcpkg and ports build with a modern C++ compiler
ENV PATH=/opt/rh/gcc-toolset-12/root/usr/bin:/opt/bin:$PATH
ENV LD_LIBRARY_PATH=/opt/rh/gcc-toolset-12/root/usr/lib64:/opt/rh/gcc-toolset-12/root/usr/lib

# Bootstrap vcpkg; project dependencies are resolved from vcpkg.json (manifest mode).
# oraclelinux:8 glibc is too old for vcpkg's prebuilt tool, so build it from source.
ENV VCPKG_FORCE_SYSTEM_BINARIES=1
RUN git clone https://github.com/microsoft/vcpkg /opt/vcpkg && \
    /opt/vcpkg/bootstrap-vcpkg.sh -disableMetrics

ENV VCPKG_ROOT=/opt/vcpkg
ENV PATH=/opt/vcpkg:$PATH
