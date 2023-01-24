FROM ubuntu:22.04

ARG GCC_VER=11.3.0
ARG BINUTILS_VER=2.40
ARG SDK_PATH="/rpizero-sdk"

RUN apt-get update && apt-get install --force-yes -y --no-install-recommends \
    build-essential \
    sudo \
    cmake \
    libmpc-dev \
    libisl-dev \
    zlib1g-dev \
    texinfo \
    gcc-multilib
#    gcc-arm-linux-gnueabihf \
#    g++-arm-linux-gnueabihf

ADD https://ftp.gnu.org/gnu/binutils/binutils-$BINUTILS_VER.tar.xz /tmp/binutils.tar.xz
ADD https://ftp.gnu.org/gnu/gcc/gcc-11.3.0/gcc-$GCC_VER.tar.xz /tmp/gcc.tar.xz
#ADD https://ftp.gnu.org/gnu/gcc/gcc-8.5.0/gcc-8.5.0.tar.xz /tmp/gcc.tar.xz
WORKDIR /tmp
RUN tar -xf gcc.tar.xz
RUN tar -xf binutils.tar.xz

WORKDIR /tmp/binutils-$BINUTILS_VER
#RUN mkdir objdir
RUN ./configure --prefix=$SDK_PATH \
            --target=arm-linux-gnueabihf \
            --enable-gold=yes \
            --enable-ld=yes \
            --enable-targets=arm-linux-gnueabihf \
            --enable-multilib \
            --enable-interwork \
            --disable-werror
RUN make -j12 && make install


WORKDIR /tmp/gcc-$GCC_VER
RUN mkdir objdir
WORKDIR /tmp/gcc-$GCC_VER/objdir

#
#RUN ../configure --program-suffix=rpizero --with-arch=armv6 --with-float=hard --with-fpu=vfp
RUN ../configure --enable-languages=c,c++  --prefix=$SDK_PATH --with-gcc-major-version-only --program-suffix=-11 --program-prefix=arm-linux-gnueabihf- --enable-shared --enable-linker-build-id --libexecdir=/usr/lib --without-included-gettext --enable-threads=posix --libdir=/usr/lib --enable-nls --enable-bootstrap --enable-clocale=gnu --enable-libstdcxx-debug --enable-libstdcxx-time=yes --with-default-libstdcxx-abi=new --enable-gnu-unique-object --disable-libitm --disable-libquadmath --disable-libquadmath-support --enable-plugin --with-system-zlib --enable-libphobos-checking=release --with-target-system-zlib=auto --enable-objc-gc=auto --enable-multiarch --disable-sjlj-exceptions --with-arch=armv6 --with-fpu=vfp --with-float=hard --disable-werror --enable-checking=release --build=arm-linux-gnueabihf --host=arm-linux-gnueabihf --target=arm-linux-gnueabihf
RUN make -j12 BOOT_CFLAGS='-O' bootstrap-lean
