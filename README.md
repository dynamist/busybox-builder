# Busybox Builder

Statically build Busybox with SELinux support.

    $ make -C alpine
    $ make -C centos
    $ make -C ubuntu

Find the binary in e.g. `./alpine/busybox`.

This is the result:

    $ file */busybox
    alpine/busybox: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, stripped
    centos/busybox: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), statically linked, for GNU/Linux 2.6.32, BuildID[sha1]=8654e5d47bd4c6f95bbd2525fd95c6a4acda94c1, stripped
    ubuntu/busybox: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), statically linked, for GNU/Linux 3.2.0, BuildID[sha1]=89ea53f99378bee7c21d4af2f8f3d17f4e855569, stripped
    $ ldd */busybox
    alpine/busybox:
            statically linked
    centos/busybox:
            not a dynamic executable
    ubuntu/busybox:
            not a dynamic executable
