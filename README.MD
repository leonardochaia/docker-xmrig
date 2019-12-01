# Docker xmrig

Run `xmrig` using Docker.

This image clones `https://github.com/xmrig/xmrig/` and builds it according to instructions.

## Usage

Append any `xmrig` params after the image name. i.e:

```shell
docker run -it --rm lchaia/xmrig --url=YOUR_POOL_URL --coin=monero # ..
```

## Docker Compose example

```yaml
version: "3"

services:
  xmrig:
    image: lchaia/xmrig
    command: 
      - "--coin=monero"
      - "--url=YOUR_POOL_URL"
      - "--user=YOUR_XMR_ADDRESS"
      - "--pass=YOUR_WORKER_ID:YOUR_EMAIL" # might change depending on pool?
      - "--rig-id=YOUR_WORKER_ID"
      - "--donate-level=0"
      - "--api-worker-id=YOUR_WORKER_ID"
      - "--http-host=0.0.0.0"
      - "--http-port=8080"
      - "--threads=0" 
```

## Copyrights

Code from this repository is released under MIT license.
xmrig code is released under GPL-3

## Donations

Please consider [donating to the xmrig team](https://github.com/xmrig/xmrig#donations).
