FROM ubuntu:jammy

ENV DEBIAN_FRONTEND=noninteractive

RUN apt update -y && \
    apt install --no-install-recommends build-essential python3 python-is-python3 \
                    qtbase5-dev qt5-qmake qtbase5-dev-tools qttools5-dev qttranslations5-l10n \
                    libboost-dev libssl-dev libcurl4-openssl-dev \
                    ragel ninja-build zlib1g-dev git pkg-config \
                    zip unzip curl autoconf automake autoconf-archive libtool \
                    wget ca-certificates fuse file -y && \
    apt clean && rm -rf /var/lib/apt/lists/*

RUN wget https://github.com/Kitware/CMake/releases/download/v3.30.5/cmake-3.30.5-linux-x86_64.sh && \
    chmod 755 cmake-3.30.5-linux-x86_64.sh && \
    ./cmake-3.30.5-linux-x86_64.sh --prefix=/opt/ --exclude-subdir --skip-license && \
    rm cmake-3.30.5-linux-x86_64.sh

# Bootstrap vcpkg; project dependencies are resolved from vcpkg.json (manifest mode)
RUN git clone https://github.com/microsoft/vcpkg /opt/vcpkg && \
    /opt/vcpkg/bootstrap-vcpkg.sh -disableMetrics

ENV VCPKG_ROOT=/opt/vcpkg
ENV PATH=/opt/vcpkg:/opt/bin:$PATH
