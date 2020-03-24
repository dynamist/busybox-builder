FROM centos:7

LABEL maintainer="Henrik Holmboe <henrik@dynamist.se>"

# https://git.busybox.net/busybox
ENV BUSYBOX_REPO "git://git.busybox.net/busybox"
ENV BUSYBOX_TAG  "1_30_1"

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

# Avoid Kconfig outputting the timestamp into generated configuration files. We
# use this to compare against the configuration files in debian/config/pkg for
# builds on Linux so that we can detect upstream configuration changes (e.g.
# added applets).
ENV KCONFIG_NOTIMESTAMP "1"

COPY config .config
RUN yes "" | make oldconfig
RUN cp -a .config /artifacts/config
RUN make
RUN cp -av busybox /artifacts

WORKDIR /artifacts
RUN ./busybox && file busybox
