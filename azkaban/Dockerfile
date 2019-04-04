FROM openjdk:8

RUN cd ~; \
    wget -O azkanbn-3.70.1.tar.gz -N https://github.com/azkaban/azkaban/archive/3.70.1.tar.gz; \
    tar -zxvf ./azkanbn-3.70.1.tar.gz -C azkaban; \
    cd ./azkanbn; \
    ./gradlew build installDist

EXPOSE 8443

CMD ["~/azkanbn/azkaban-solo-server/build/install/azkaban-solo-server/bin/start-solo.sh"]

