ARG ALPINE_VERSION=3.10
FROM lchaia/alpine-gcc:7.4.0 as builder

RUN apk --no-cache add \
    libuv-dev \
    openssl-dev \
    libmicrohttpd \
    git \
    cmake

RUN apk --no-cache add \
    --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing \
    hwloc-dev

ARG XMRIG_VERSION=v5.5.3
ARG XMRIG_BUILD_ARGS="-DCMAKE_BUILD_TYPE=Release"

WORKDIR /xmrig
# Clone XMRIG
RUN git clone -b ${XMRIG_VERSION} --single-branch --depth 1 https://github.com/xmrig/xmrig ./ && \
    # I'm sorry :(
    sed -i 's/kMinimumDonateLevel = 1;/kMinimumDonateLevel = 0;/g' ./src/donate.h && \
    mkdir build && \
    cmake ${XMRIG_BUILD_ARGS} . && \
    make && \
    ./xmrig --help

FROM alpine:${ALPINE_VERSION}

COPY --from=builder /xmrig/xmrig /bin/

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