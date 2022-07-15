ARG ALPINE_VERSION=3.10
FROM lchaia/alpine-gcc:7.4.0 as builder

RUN apk add git make cmake libstdc++ gcc g++ automake libtool autoconf linux-headers

ARG XMRIG_VERSION=v6.18.0
ARG XMRIG_BUILD_ARGS="-DCMAKE_BUILD_TYPE=Release"

WORKDIR /xmrig
# Clone & build XMRIG for alpine according to official docs
# source: https://xmrig.com/docs/miner/build/alpine
RUN git clone -b ${XMRIG_VERSION} --single-branch --depth 1 https://github.com/xmrig/xmrig ./ && \
    # I'm sorry :(
    sed -i 's/kMinimumDonateLevel = 1;/kMinimumDonateLevel = 0;/g' ./src/donate.h && \
    mkdir build && \
    cd scripts && ./build_deps.sh && cd ../build && \
    cmake .. -DXMRIG_DEPS=scripts/deps -DBUILD_STATIC=ON && \
    make -j$(nproc) && \
    ./xmrig --help

RUN ls ./build -l

FROM alpine:${ALPINE_VERSION}

COPY --from=builder /xmrig/build/xmrig /bin/

RUN adduser -S -D -H -h /xmrig miner
RUN xmrig --help

USER miner

ENTRYPOINT ["xmrig"]
CMD [ "--help" ]