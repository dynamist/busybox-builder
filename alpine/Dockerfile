FROM alpine:3.8

LABEL maintainer="Henrik Holmboe <henrik@dynamist.se>"

ENV BUSYBOX_REPO "git://git.busybox.net/busybox"
ENV BUSYBOX_TAG  "1_29_2"

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

COPY config .config
RUN make
RUN ./busybox && file busybox && cp -a busybox /artifacts
