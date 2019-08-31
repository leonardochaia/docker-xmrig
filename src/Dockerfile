## GCC Build setup from https://github.com/n0madic/alpine-gcc/blob/master/Dockerfile
ARG ALPINE_VERSION=3.10
FROM alpine:${ALPINE_VERSION} as builder-gcc

ARG GCC_VERSION=7.4.0
ENV GCC_VERSION=${GCC_VERSION}

RUN apk add --quiet --no-cache \
    build-base \
    dejagnu \
    isl-dev \
    make \
    mpc1-dev \
    mpfr-dev \
    texinfo \
    zlib-dev

RUN wget -q https://ftp.gnu.org/gnu/gcc/gcc-${GCC_VERSION}/gcc-${GCC_VERSION}.tar.gz && \
    tar -xzf gcc-${GCC_VERSION}.tar.gz && \
    rm -f gcc-${GCC_VERSION}.tar.gz

WORKDIR /gcc-${GCC_VERSION}

RUN ./configure \
    --prefix=/usr/local \
    --build=$(uname -m)-alpine-linux-musl \
    --host=$(uname -m)-alpine-linux-musl \
    --target=$(uname -m)-alpine-linux-musl \
    --with-pkgversion="Alpine ${GCC_VERSION}" \
    --enable-checking=release \
    --disable-fixed-point \
    --disable-libmpx \
    --disable-libmudflap \
    --disable-libsanitizer \
    --disable-libssp \
    --disable-libstdcxx-pch \
    --disable-multilib \
    --disable-nls \
    --disable-symvers \
    --disable-werror \
    --enable-__cxa_atexit \
    --enable-default-pie \
    --enable-languages=c,c++ \
    --enable-shared \
    --enable-threads \
    --enable-tls \
    --with-linker-hash-style=gnu \
    --with-system-zlib

RUN make --silent -j $(nproc)
RUN make --silent -j $(nproc) install-strip

RUN gcc -v

FROM alpine:${ALPINE_VERSION} as builder-xmrig

COPY --from=builder-gcc /usr/local/ /usr/

RUN apk --no-cache add \
    --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing \
    hwloc-dev

RUN apk --no-cache add \
    libuv-dev \
    openssl-dev \
    libmicrohttpd \
    git \
    cmake \
    autoconf \
    automake \
    binutils \
    file \
    git \
    gmp \
    isl \
    libc-dev \
    libtool \
    make \
    mpc1 \
    mpfr3 \
    musl-dev \
    pkgconf \
    zlib-dev

ARG XMRIG_VERSION=v3.1.1
ARG XMRIG_BUILD_ARGS="-DCMAKE_BUILD_TYPE=Release"

# Clone XMRIG
RUN git clone -b ${XMRIG_VERSION} --single-branch --depth 1 https://github.com/xmrig/xmrig && \
    cd xmrig && \
    # I'm sorry :(
    sed -i 's/kMinimumDonateLevel = 1;/kMinimumDonateLevel = 0;/g' ./src/donate.h && \
    mkdir build && \
    cmake ${XMRIG_BUILD_ARGS} . && \
    make && \
    ./xmrig --help

FROM alpine:${ALPINE_VERSION}

COPY --from=builder-xmrig /xmrig/xmrig /bin/

RUN apk --no-cache add \
    libuv-dev 

RUN apk --no-cache add \
    --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing \
    hwloc-dev

RUN adduser -S -D -H -h /xmrig miner
RUN xmrig --help

USER miner

ENTRYPOINT ["xmrig"]
CMD [ "--help" ]