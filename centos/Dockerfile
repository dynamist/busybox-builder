FROM centos:7

LABEL maintainer="Henrik Holmboe <henrik@dynamist.se>"

# https://git.busybox.net/busybox
ENV BUSYBOX_REPO "git://git.busybox.net/busybox"
ENV BUSYBOX_TAG  "1_29_2"

RUN yum groupinstall -y "Development Tools" \
    && yum clean all

RUN yum install -y \
    ncurses-devel \
    libselinux-static \
    libsepol-static \
    glibc-static \
    pcre-static \
    && yum clean all

RUN mkdir -p /build /artifacts
WORKDIR /build

RUN git clone $BUSYBOX_REPO --branch $BUSYBOX_TAG --depth 1
WORKDIR /build/busybox

COPY config .config
RUN make
RUN cp -av busybox /artifacts

WORKDIR /artifacts
RUN ./busybox && file busybox
