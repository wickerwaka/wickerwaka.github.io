+++
title = "68000 Build Environment"
date = "2023-06-03T11:29:24-07:00"
draft = false
description = "Setting up a C compiler for F2 test ROM creation."
+++

I want to develop the F2 test ROMs in C. The M92 test ROMs were written in x86 assembly because there wasn't an obvious golden path for getting a 16-bit x86 toolchain working on modern hardware. The 68000 architecture is still used today in embedded systems, and it is still supported by the GNU C Compiler as the `m68k-elf` target.

Needing to build a cross-compiler is usually the point at which I abandon a project. Experiences have taught me that it is a poorly documented, error-prone and slow process. Those first two points are still largely true. I found myself googling random error strings. Finding snippets of info in forums and articles without much context as to why a particular option or setting was used. However, it is not a slow process anymore! Both my laptop and desktop PC can do a full GCC rebuild in less than two minutes.

I have a basic set of needs and no not need much support beyond bare C. I don't need any floating point math or file IO. I need to do some basic string formatting, but I don't want to deal with dynamic memory allocation, so I will use a custom `sprintf` [implementation](https://github.com/eyalroz/printf). So, all this to say I don't need a `libc` implementation and I won't be trying to build [newlib](https://sourceware.org/newlib/) or [musl](https://musl.libc.org/). I will happily grab snippets from those libraries when needed.

## Building the toolchain
The most concise set of build steps I found was from [this guide](http://www.aaldert.com/outrun/gcc.html) for building an Outrun arcade toolchain. I've created my own set of steps here which should work on Linux (I'm using WSL2 on Windows) and MacOS. On a clean system you will likely run into some missing dependencies during the configuration step, and you will have to install them with your package manager of choice.

On Mac I use [Homebrew](https://brew.sh/) which will install some alternative versions of libraries into `/opt/homebrew`. If you get errors related to missing libraries (`libgmp` for example) that you know you have installed you can point `configure` to the correct location with `--with-gmp=/opt/homebrew`.

The script below will download and build GCC in a directory called `m68k-gcc`. I don't recommend running it as is. Use it as a reference, which is what I will do several months from now when I forget how to do it.

```bash
# Create a working directory
mkdir m68k-gcc
cd m68k-gcc

# Download latest versions of binutils and GCC
curl https://ftp.gnu.org/gnu/binutils/binutils-2.40.tar.xz -s | tar Jxv
curl https://ftp.gnu.org/gnu/gcc/gcc-13.1.0/gcc-13.1.0.tar.xz -s | tar Jxv

# Make build and output directories
mkdir -p build/binutils build/gcc toolchain

# Build binutils
cd build/binutils
../../binutils-2.40/configure \
    --prefix=$(pwd)/../../toolchain/ \
    --target=m68k-elf
make -j8 && make install

# Build GCC
cd ../gcc
../../gcc-13.1.0/configure \
    --prefix=$(pwd)/../../toolchain/ \
    --target=m68k-elf \
    --enable-languages=c \
    --disable-libmudflap \
    --disable-libssp \
    --disable-libgomp \
    --disable-libstdcxx-pch \
    --disable-threads \
    --disable-nls \
    --disable-libquadmath \
    --with-gnu-as \
    --with-gnu-ld \
    --without-headers

make -j8 && make install
```









