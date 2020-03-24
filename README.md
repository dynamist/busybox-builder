# Busybox Builder

Statically build Busybox with SELinux support.

    $ make -C alpine
    $ make -C centos
    $ make -C ubuntu

Find the binary in e.g. `./alpine/busybox`.

This is the result:

```console
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
```

Sizes of Busybox version 1.31.1:

```console
$ size */busybox
   text    data     bss     dec     hex filename
1732134   17383   20377 1769894  1b01a6 alpine/busybox
3393165    9045   46411 3448621  349f2d centos/busybox
3673677   39721   43803 3757201  395491 ubuntu/busybox
```
