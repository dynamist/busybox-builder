FROM ubuntu:18.04

LABEL maintainer="Henrik Holmboe <henrik@dynamist.se>"

# https://git.busybox.net/busybox
ENV BUSYBOX_REPO "git://git.busybox.net/busybox"
ENV BUSYBOX_TAG  "1_30_0"

RUN apt-get update \
    && apt-get install -y \
       build-essential \
       git \
       pkg-config \
       file \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
    && apt-get install -y \
       libncurses5-dev \
       libselinux1-dev \
       libsepol1-dev \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /build /artifacts
WORKDIR /build

RUN git clone $BUSYBOX_REPO --branch $BUSYBOX_TAG --depth 1
WORKDIR /build/busybox

# Fix bug in Busybox 1.30.0
RUN sed -i 's/ACTION_RECURSIVE/ACTION_RECURSE/' selinux/chcon.c

# Avoid Kconfig outputting the timestamp into generated configuration files. We
# use this to compare against the configuration files in debian/config/pkg for
# builds on Linux so that we can detect upstream configuration changes (e.g.
# added applets).
ENV KCONFIG_NOTIMESTAMP "1"

COPY config .config
RUN make oldconfig
RUN cp -a .config /artifacts/config
RUN make
RUN cp -av busybox /artifacts

WORKDIR /artifacts
RUN ./busybox && file busybox
