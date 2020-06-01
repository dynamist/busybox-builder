FROM alpine:3.12.0

LABEL maintainer="Henrik Holmboe <henrik@dynamist.se>"

# https://git.busybox.net/busybox
ENV BUSYBOX_REPO "git://git.busybox.net/busybox"
ENV BUSYBOX_TAG  "1_31_1"

# https://git.alpinelinux.org/cgit/aports/tree/main/alpine-mirrors/mirrors.yaml
RUN echo "@testing https://alpine.mirror.wearetriple.com/edge/testing" >> /etc/apk/repositories

RUN apk update && \
    apk add \
        build-base \
        ncurses-dev \
        linux-headers \
        git

RUN apk add \
    libselinux-dev@testing \
    libsepol-dev@testing

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
RUN cp -av .config /artifacts/config
RUN make
RUN cp -av busybox /artifacts

WORKDIR /artifacts
RUN ./busybox && file busybox
