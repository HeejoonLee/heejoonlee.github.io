---
title: "[Code Basics] Library Linking"
date: 2023-11-26 03:46 +0900
categories: [Code Basics, Linux]
tags: [Linux, kernel, gcc, linking]
---

This post explains how external libraries are linked by `gcc`.

### Default `gcc` Options

We will first try to compile a simple C source file to check the default options used by `gcc` toolchain.

```c
// main.c
int main(int argc, char *argv[])
{
    return 0;
}
```
```
$ gcc -o main main.c
```

Successful completion of the above command generates a single executable file named `main`, which when run, exits without showing any output on the terminal. 

We can use the `ldd` command to check all the dynamic libraries linked to an executable:

```
$ ldd main
        linux-vdso.so.1 (0x00007fff327f4000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007efc97e10000)
        /lib64/ld-linux-x86-64.so.2 (0x00007efc98044000)
```

The output above shows 3 lines. The first shared object(.so) file, `linux-vdso.so.1` is a *vDSO* file which is a library that is automatically included to facilitate system calls. This post will not deal with this .so file any further; it is enough to note that `linux-vdso.so.1` will be included with every dynamically-linked executable file.

The last line `/lib64/ld-linux-x86-64.so.2` is the dynamic linker which is used to run dynamically-linked files.

The actual dynamic library linked in `main` is the `libc.so.6` which is located in `/lib/x86_64-linux-gnu/libc.so.6` in the filesystem.

It may be quite useful to venture further and find out how `libc.so.6` ended up being included in the final executable file although we never specified anywhere(either in the source or gcc option) to include that file.

Adding the *-v* option in `gcc` reveals more of what was actually executed:

```
$ gcc -v -o main main.c
Using built-in specs.
...
```

Since the output is quite long, we will divide the output into 3 parts.

#### Compiler: `cc1`
```
gcc version 11.4.0 (Ubuntu 11.4.0-1ubuntu1~22.04) 
COLLECT_GCC_OPTIONS='-v' '-o' 'main' '-mtune=generic' '-march=x86-64'
 /usr/lib/gcc/x86_64-linux-gnu/11/cc1 -quiet -v -imultiarch x86_64-linux-gnu main.c -quiet -dumpbase main.c -dumpbase-ext .c -mtune=generic -march=x86-64 -version -fasynchronous-unwind-tables -fstack-protector-strong -Wformat -Wformat-security -fstack-clash-protection -fcf-protection -o /tmp/ccZazD5D.s
GNU C17 (Ubuntu 11.4.0-1ubuntu1~22.04) version 11.4.0 (x86_64-linux-gnu)
        compiled by GNU C version 11.4.0, GMP version 6.2.1, MPFR version 4.1.0, MPC version 1.2.1, isl version isl-0.24-GMP

GGC heuristics: --param ggc-min-expand=100 --param ggc-min-heapsize=131072
ignoring nonexistent directory "/usr/local/include/x86_64-linux-gnu"
ignoring nonexistent directory "/usr/lib/gcc/x86_64-linux-gnu/11/include-fixed"
ignoring nonexistent directory "/usr/lib/gcc/x86_64-linux-gnu/11/../../../../x86_64-linux-gnu/include"
#include "..." search starts here:
#include <...> search starts here:
 /usr/lib/gcc/x86_64-linux-gnu/11/include
 /usr/local/include
 /usr/include/x86_64-linux-gnu
 /usr/include
End of search list.
```

The output of the gcc compiler `cc1` is shown above. We can see that the actual path of the executable is `/usr/lib/gcc/x86_64-linux-gnu/11/cc1` with many default flags that were used to compile the source file. Note also that the input file is specified as `main.c`, but the output as `-o /tmp/ccZazD5D.s`. 

The options or arguments we passed to `gcc` may not all be used by the compiler; some may be used by other programs in the toolchain as will be explained below.

We can also check the default search paths that the compiler uses when we specify header files using the `#include <...>` format.

#### Assembler: `as`
```
COLLECT_GCC_OPTIONS='-v' '-o' 'main' '-mtune=generic' '-march=x86-64'
 as -v --64 -o /tmp/ccwGdyaH.o /tmp/ccZazD5D.s
GNU assembler version 2.38 (x86_64-linux-gnu) using BFD version (GNU Binutils for Ubuntu) 2.38
```

The gcc assembler `as` assembles the *.s* assembly file into the object file `/tmp/ccwGdyaH.o`

#### Linker: `collect2`
```
COMPILER_PATH=/usr/lib/gcc/x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/
LIBRARY_PATH=/usr/lib/gcc/x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../../lib/:/lib/x86_64-linux-gnu/:/lib/../lib/:/usr/lib/x86_64-linux-gnu/:/usr/lib/../lib/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../:/lib/:/usr/lib/
COLLECT_GCC_OPTIONS='-v' '-o' 'main' '-mtune=generic' '-march=x86-64' '-dumpdir' 'main.'
 /usr/lib/gcc/x86_64-linux-gnu/11/collect2 -plugin /usr/lib/gcc/x86_64-linux-gnu/11/liblto_plugin.so -plugin-opt=/usr/lib/gcc/x86_64-linux-gnu/11/lto-wrapper -plugin-opt=-fresolution=/tmp/cci2UbDA.res -plugin-opt=-pass-through=-lgcc -plugin-opt=-pass-through=-lgcc_s -plugin-opt=-pass-through=-lc -plugin-opt=-pass-through=-lgcc -plugin-opt=-pass-through=-lgcc_s --build-id --eh-frame-hdr -m elf_x86_64 --hash-style=gnu --as-needed -dynamic-linker /lib64/ld-linux-x86-64.so.2 -pie -z now -z relro -o main /usr/lib/gcc/x86_64-linux-gnu/11/../../../x86_64-linux-gnu/Scrt1.o /usr/lib/gcc/x86_64-linux-gnu/11/../../../x86_64-linux-gnu/crti.o /usr/lib/gcc/x86_64-linux-gnu/11/crtbeginS.o -L/usr/lib/gcc/x86_64-linux-gnu/11 -L/usr/lib/gcc/x86_64-linux-gnu/11/../../../x86_64-linux-gnu -L/usr/lib/gcc/x86_64-linux-gnu/11/../../../../lib -L/lib/x86_64-linux-gnu -L/lib/../lib -L/usr/lib/x86_64-linux-gnu -L/usr/lib/../lib -L/usr/lib/gcc/x86_64-linux-gnu/11/../../.. /tmp/ccwGdyaH.o -lgcc --push-state --as-needed -lgcc_s --pop-state -lc -lgcc --push-state --as-needed -lgcc_s --pop-state /usr/lib/gcc/x86_64-linux-gnu/11/crtendS.o /usr/lib/gcc/x86_64-linux-gnu/11/../../../x86_64-linux-gnu/crtn.o
COLLECT_GCC_OPTIONS='-v' '-o' 'main' '-mtune=generic' '-march=x86-64' '-dumpdir' 'main.'
```
This is the last step of gcc compilation, the linking step. We can see that the name of the linker executable is `collect2` with various options following it. What we need to focus on are the flags `-L` and `-l`, which tell the linker where and which library functions to include in the final executable object file.

`-L<path>` tells the linker that a library file *may be* located in the directory pointed by *path* and should search that directory.  `-L<path>` is a shorthand form of `--library-path=<path>`.

`-l<lib>` indicates the actual name of the library file, `llib.so` for dynamic, and `llib.a` for static library file. `gcc` is configured to look for `.so` dynamic library files by default; if both `.so` and `.a` exist with the same name, the `.so` file will be linked. `-l<lib>` is a shorthand form of `--library=<lib>`

One other flag to note is the `--as-needed` flag which tells the linker to only include the dynamic library appearing right after it *if it is needed*. 

Going back to the output above, the dynamic libraries to be linked are the `libgcc.a`(from `-lgcc`) and `libc.so`(from `-lc`). (`-lgcc_s` is optional, so it may be not be linked if not needed).

A simple search in the root filesystem lets us point out where exactly the dynamic library files are. 

`libc.so` is located in `/lib/x86_64-linux-gnu/libc.so.6`. This is also apparent from the `ldd` output.

`libgcc.a` is located in `/usr/lib/gcc/x86_64-linux-gnu/11/libgcc.a`. It turned out that the `libgcc` is in fact a *static library*, which is the reason why `libgcc` did not show up in the `ldd` output.

Do note that the directories in which the two library files reside were specified using the `-L` flag.

> We can also see that although the output from `as` is only `/tmp/ccwGdyaH.o`, it is linked with two other object files, `crtendS.o` and `crtn.o`, to produce the final exectuable file `main`. 

### Compiling And Using a Custom Static Library

### Compiling And Using a Custom Dynamic Library

>> Second source file which shows the use of external library(libquadmath), Compiling ans using static library, Compiling and using dynamic library

