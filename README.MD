# Docker xmrig

Run `xmrig` using Docker.

This image clones `https://github.com/xmrig/xmrig/` and builds it according to instructions.

## Usage

Append any `xmrig` params after the image name. i.e:

```shell
docker run -it --rm lchaia/xmrig --url randomx-benchmark.xmrig.com:7777 --algo rx/test
```

## Docker Compose example

```yaml
version: "3"

services:
  xmrig:
    image: lchaia/xmrig
    command: -c /xmrig/config.json --url randomx-benchmark.xmrig.com:7777 --algo rx/test
    volumes:
      - ./xmrig-config.json:/xmrig/config.json
```

## Copyrights

Code from this repository is released under MIT license.
xmrig code is released under GPL-3

## Donations

Please consider [donating to the xmrig team](https://github.com/xmrig/xmrig#donations).
