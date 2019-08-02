# Scrapydweb

**Note:**

Default scrapydweb not install scrapyd and logparser ! So you should
use `-ss scrapyd:6800` to override default parameter when start container.

You should deploy [crapyd](https://scrapyd.readthedocs.io/en/stable/) and [logparser](https://github.com/my8100/logparser),
before start scrapydweb. or use [scrapyd-logparser](https://hub.docker.com/r/huagang/scrapyd-logparser).

Default scrapydweb not install scrapyd and logparser ! So you should
use `-ss scrapyd:6800` to override default parameter when start container.

If you use docker compose, zhe [docker compose file](./docker-compose.yml) can be used.

## Usage

```bash
docker run --name scrapydweb -p 5000:5000 huagang/scrapyd:latest -ss scrapyd:6800
```

## Config file

1. scrapydweb conf

    location: ~/scrapydweb_settings_v8.py

    Scrapydweb 默认使用本地(127.0.0.1) 的 Scrapyd
