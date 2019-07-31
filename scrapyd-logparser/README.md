# Scrapyd-logparser

![crapyd](https://scrapyd.readthedocs.io/en/stable/)
![logparser](https://github.com/my8100/logparser)

## Usage

```bash
docker run --name scrapyd_logparser -p 6800:6800 scrapyd-logparser:latest
```

### volume

You can use volume `eggs` to share Scrapyd egg file between scrapyd-logparser cluster.

```bash
docker run --name scrapyd_logparser -p 6800:6800 -v eggs:/app/eggs scrapyd-logparser:latest
```
