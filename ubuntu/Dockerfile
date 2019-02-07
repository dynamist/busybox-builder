FROM ubuntu:18.04

LABEL maintainer="Henrik Holmboe <henrik@dynamist.se>"

# https://git.busybox.net/busybox
ENV BUSYBOX_REPO "git://git.busybox.net/busybox"
ENV BUSYBOX_TAG  "1_29_2"

RUN apt update \
    && apt install -y build-essential git pkg-config file \
    && rm -rf /var/lib/apt/lists/*

RUN apt update \
    && apt install -y \
       libncurses5-dev \
       libselinux1-dev \
       libsepol1-dev \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /build /artifacts
WORKDIR /build

RUN git clone $BUSYBOX_REPO --branch $BUSYBOX_TAG --depth 1
WORKDIR /build/busybox

COPY config .config
RUN make
RUN cp -av busybox /artifacts

WORKDIR /artifacts
RUN ./busybox && file busybox
