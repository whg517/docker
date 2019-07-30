FROM python:3.6

WORKDIR /app

RUN pip install -i https://mirrors.aliyun.com/pypi/simple/ -U pip \
    && pip install  -i https://mirrors.aliyun.com/pypi/simple/ -U supervisor \
    && pip install -i https://mirrors.aliyun.com/pypi/simple/ -U scrapyd \
    && echo_supervisord_conf | sed -e '/^nodaemon=/s/false/true/' \
                                   -e '/^;\[include\]/s/;//' \
                                   -e 's/^;\(files = \).*/\1supervisord.d\/*.ini/g' \
                                   > /etc/supervisord.conf \
    && cat `find / -name default_scrapyd.conf | grep scrapyd` | sed 's/^\(bind_address = \).*/\10.0.0.0/g' > scrapyd.conf \
    && rm -rf ~/.cache

ADD ./supervisor_scrapyd.ini /etc/supervisord.d/scrapyd.ini

EXPOSE 6800

CMD [ "supervisord", "-c", "/etc/supervisord.conf" ]