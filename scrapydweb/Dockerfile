FROM python:3.6

WORKDIR /app

RUN pip install -i https://mirrors.aliyun.com/pypi/simple/ -U pip \
    && pip install -i https://mirrors.aliyun.com/pypi/simple/ -U scrapydweb \
    && pip install -i https://mirrors.aliyun.com/pypi/simple/ -U scrapyd \
    && cat `find / -name default_settings.py | grep scrapydweb` | sed -e '/^SCRAPYD_SERVERS/,/\]/{/^\s/d}' \
                                                                      -e '/^ENABLE_LOGPARSER /s/True/False/' \
                                                                      > scrapydweb_settings_v8.py \
    && rm -rf ~/.cache

EXPOSE 5000

ENTRYPOINT ["scrapydweb"]
CMD [ "-ss", "127.0.0.1"]
