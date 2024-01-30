ARG UBUNTU_VERSION

FROM ubuntu:${UBUNTU_VERSION} AS build

ARG SCANCONTROL_SDK_VERSION
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    wget \
    git \
    unzip 

RUN apt-get install -y --no-install-recommends \
    checkinstall \
    cmake \
    debhelper \
    g++ \
    gcc \
    gettext \
    gobject-introspection \
    gtk-doc-tools \
    intltool \
    libgirepository1.0-dev \
    libglib2.0-dev \
    libgstreamer1.0-dev \
    libgstreamer-plugins-base1.0-dev \
    libgstreamer-plugins-good1.0-dev \
    libgtk-3-dev \
    libusb-1.0-0-dev \
    libxml2-dev \
    python3-pip \
    ninja-build \
    pkg-config \
    xsltproc
RUN rm -rf /var/lib/apt/lists/*

RUN pip3 install meson

RUN wget https://github.com/AravisProject/aravis/releases/download/0.8.30/aravis-0.8.30.tar.xz -O aravis-0.8.30.tar.xz && \
    tar xfJ aravis-0.8.30.tar.xz && \
    cd aravis-0.8.30 && \
    meson setup build && \
    cd build && \
    ninja && \
    checkinstall --pkgname aravis --pkgversion 0.8.30 --requires="libglib2.0-dev, libxml2-dev, libusb-1.0-0-dev" ninja install

SHELL ["/bin/bash", "-c"]
RUN wget https://software.micro-epsilon.com/scanCONTROL-Linux-SDK-${SCANCONTROL_SDK_VERSION//./-}.zip -O scanCONTROL-Linux-SDK.zip
RUN unzip scanCONTROL-Linux-SDK.zip -d scanCONTROL-Linux-SDK/

RUN cd scanCONTROL-Linux-SDK/libmescan/ && \ 
    meson builddir && \
    cd builddir && \
    ninja && \
    checkinstall --pkgname mescan --pkgversion ${SCANCONTROL_SDK_VERSION} --requires="aravis \(\>= 0.8.0\)" ninja install
    
RUN ldconfig

RUN cd scanCONTROL-Linux-SDK/libllt && \
    meson builddir && \
    cd builddir && \
    ninja && \
    checkinstall --pkgname llt --pkgversion ${SCANCONTROL_SDK_VERSION} --requires="mescan \(\>= ${SCANCONTROL_SDK_VERSION}\),aravis \(\>= 0.8.0\)" ninja install
