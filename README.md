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
    $ for busybox in */busybox; do checksec -f $busybox; done
    RELRO           STACK CANARY      NX            PIE             RPATH      RUNPATH      Symbols         FORTIFY Fortified       Fortifiable  FILE
    Full RELRO      No canary found   NX enabled    PIE enabled     No RPATH   No RUNPATH   No Symbols      No      0               0       alpine/busybox
    RELRO           STACK CANARY      NX            PIE             RPATH      RUNPATH      Symbols         FORTIFY Fortified       Fortifiable  FILE
    Partial RELRO   No canary found   NX enabled    No PIE          No RPATH   No RUNPATH   No Symbols      No      0               0       centos/busybox
    RELRO           STACK CANARY      NX            PIE             RPATH      RUNPATH      Symbols         FORTIFY Fortified       Fortifiable  FILE
    Partial RELRO   No canary found   NX enabled    No PIE          No RPATH   No RUNPATH   No Symbols      No      0               0       ubuntu/busybox

Sizes of Busybox version 1.31.1:

    $ for busybox in */busybox; do size $busybox; done
       text    data     bss     dec     hex filename
    1731941   17383   20377 1769701  1b00e5 alpine/busybox
       text    data     bss     dec     hex filename
    3392984    9045   46411 3448440  349e78 centos/busybox
       text    data     bss     dec     hex filename
    3673481   39721   43803 3757005  3953cd ubuntu/busybox
