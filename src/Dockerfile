FROM alpine:3.10

RUN adduser -S -D -H -h /xmrig miner

# Install xmrig Dependencies
RUN apk --no-cache add \
    --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing \
    libuv-dev \
    openssl-dev \
    libmicrohttpd \ 
    hwloc-dev

RUN apk --no-cache add \
    git \
    cmake \
    build-base && \
    # Clone XMRIG
    git clone https://github.com/xmrig/xmrig && \
    cd xmrig && \
    # I'm sorry :(
    sed -i 's/kMinimumDonateLevel = 1;/kMinimumDonateLevel = 0;/g' ./src/donate.h && \
    mkdir build && \
    cmake -DCMAKE_BUILD_TYPE=Release . && \
    make && \
    apk del \
    build-base \
    cmake \
    git

USER miner
WORKDIR /xmrig

ENTRYPOINT ["./xmrig"]