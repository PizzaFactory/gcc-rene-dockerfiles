FROM eclipse/cpp_gcc

ENV DEBIAN_FRONTEND noninteractive
ENV PATH $PATH:/opt/gcc-rene/bin

ARG GCC_RENE_RELEASE=16.01
ARG BINUTILS_VERSION=2.25.90-20151217
ARG GCC_VERSION=5.2.1-20151202
ARG NEWLIB_VERSION=2.2.0
ARG GDB_VERSION=7.8.2
ARG BINARY_TARGET=arm-none

USER root

RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install -y build-essential libgmp-dev libmpfr-dev libmpc-dev texinfo bison flex

SHELL [ "/bin/bash", "-c" ]

WORKDIR /opt/sources
RUN wget -q https://gcc-renesas.com/downloads/d.php?f=${BINARY_TARGET}/binutils/${GCC_RENE_RELEASE}/binutils-${BINUTILS_VERSION}_${BINARY_TARGET/-/_}_${GCC_RENE_RELEASE}.tar.bz2 -O binutils-${BINUTILS_VERSION}_${BINARY_TARGET}_${GCC_RENE_RELEASE}.tar.bz2
RUN wget -q https://gcc-renesas.com/downloads/d.php?f=${BINARY_TARGET}/gcc/${GCC_RENE_RELEASE}/gcc-${GCC_VERSION}_${BINARY_TARGET/-/_}_${GCC_RENE_RELEASE}.tar.bz2 -O gcc-${GCC_VERSION}_${BINARY_TARGET}_${GCC_RENE_RELEASE}.tar.bz2
RUN wget -q https://gcc-renesas.com/downloads/d.php?f=${BINARY_TARGET}/newlib/${GCC_RENE_RELEASE}/newlib-${NEWLIB_VERSION}_${BINARY_TARGET/-/_}_${GCC_RENE_RELEASE}.tar.bz2 -O newlib-${NEWLIB_VERSION}_${BINARY_TARGET}_${GCC_RENE_RELEASE}.tar.bz2
RUN wget -q https://gcc-renesas.com/downloads/d.php?f=${BINARY_TARGET}/gdb/${GCC_RENE_RELEASE}/gdb-${GDB_VERSION}_${BINARY_TARGET/-/_}_${GCC_RENE_RELEASE}.tar.bz2 -O gdb-${GDB_VERSION}_${BINARY_TARGET}_${GCC_RENE_RELEASE}.tar.bz2

WORKDIR /tmp/sources
RUN tar xf /opt/sources/binutils-${BINUTILS_VERSION}_${BINARY_TARGET}_${GCC_RENE_RELEASE}.tar.bz2
RUN tar xf /opt/sources/gcc-${GCC_VERSION}_${BINARY_TARGET}_${GCC_RENE_RELEASE}.tar.bz2
RUN tar xf /opt/sources/newlib-${NEWLIB_VERSION}_${BINARY_TARGET}_${GCC_RENE_RELEASE}.tar.bz2
RUN tar xf /opt/sources/gdb-${GDB_VERSION}_${BINARY_TARGET}_${GCC_RENE_RELEASE}.tar.bz2
RUN find /tmp/sources/gdb-${GDB_VERSION} -name compile -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name config.guess -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name config.rpath -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name config.sub -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name configure -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name depcomp -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name install-sh -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name missing -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name mkdep -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name mkinstalldirs -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name move-if-change -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name symlink-tree -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name ylwrap -exec chmod 0775 {} \;
RUN find /tmp/sources/gdb-${GDB_VERSION} -name '*.sh' -exec chmod 0775 {} \;

WORKDIR /tmp/build/binutils
RUN /tmp/sources/binutils-${BINUTILS_VERSION}/configure --target=${BINARY_TARGET}-eabi --prefix=/opt/gcc-rene --enable-soft-float
RUN make CFLAGS="-Wno-cast-function-type -Wno-implicit-fallthrough -Wno-shift-negative-value -Wno-unused-value -Wno-pointer-compare"
RUN make install

WORKDIR /tmp/build/gcc
RUN apt-get install -y gcc-4.9 g++-4.9
RUN CC=gcc-4.9 CXX=g++-4.9 /tmp/sources/gcc-${GCC_VERSION}/configure --target=${BINARY_TARGET}-eabi --prefix=/opt/gcc-rene --enable-languages=c,c++ --disable-nls --disable-multilib --disable-libssp --with-newlib --with-headers=/tmp/sources/newlib-${NEWLIB_VERSION}/newlib/libc/include
RUN make CXXFLAGS="-std=c++03"
RUN make install

WORKDIR /tmp/build/newlib
RUN /tmp/sources/newlib-${NEWLIB_VERSION}/configure --target=${BINARY_TARGET}-eabi --prefix=/opt/gcc-rene
RUN make
RUN make install

WORKDIR /tmp/build/gdb
RUN /tmp/sources/gdb-${GDB_VERSION}/configure --target=${BINARY_TARGET}-eabi --prefix=/opt/gcc-rene --disable-sim
RUN make
RUN make install

USER user
