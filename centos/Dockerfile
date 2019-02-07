FROM centos:7

LABEL maintainer="Henrik Holmboe <henrik@dynamist.se>"

# https://git.busybox.net/busybox
ENV BUSYBOX_REPO "git://git.busybox.net/busybox"
ENV BUSYBOX_TAG  "1_30_0"

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

# Fix bug in Busybox 1.30.0
RUN sed -i 's/ACTION_RECURSIVE/ACTION_RECURSE/' selinux/chcon.c

COPY config .config
RUN make oldconfig
RUN cp -a .config /artifacts/config
RUN make
RUN cp -av busybox /artifacts

WORKDIR /artifacts
RUN ./busybox && file busybox
