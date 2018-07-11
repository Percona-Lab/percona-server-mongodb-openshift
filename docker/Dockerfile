FROM debian:jessie
MAINTAINER Percona Development Team <opensource-dev@percona.com>

RUN apt-get update && apt-get install -y --no-install-recommends \
                apt-transport-https ca-certificates \
                pwgen \
        && rm -rf /var/lib/apt/lists/*

#RUN apt-key adv --keyserver keys.gnupg.net --recv-keys 8507EFA5

RUN echo 'deb https://repo.percona.com/apt jessie main' > /etc/apt/sources.list.d/percona.list

# the numeric UID is needed for OpenShift
RUN useradd -u 999 -r -g 0 -s /sbin/nologin \
            -c "Default Application User" mongodb

ENV PERCONA_MAJOR 36
#ENV PERCONA_VERSION 3.0.14-1.9.jessie

#        && rm -rf /data/db && mkdir -p /data/db \ 
#        && chown -R 999:0 /data/db \
# && mkdir -p /worker-dir && chown -R 999:0 /worker-dir \
# && wget -qO /worker-dir/peer-finder http://storage.googleapis.com/kubernetes-release/pets/peer-finder

RUN \
        apt-get update \
        && apt-get install -y --force-yes \
          openssl vim wget curl percona-server-mongodb-$PERCONA_MAJOR \
        && rm -rf /var/lib/apt/lists/* 

RUN  mkdir -p /data/db && chown -R 999:0 /data/db 

VOLUME ["/data/db"]

EXPOSE 27017


COPY ps-entry.sh /entrypoint.sh
# RUN chown -R 999:0 /worker-dir
ENTRYPOINT ["/entrypoint.sh"]
#RUN chmod -c 755 /worker-dir/peer-finder

USER 999

CMD ["mongod"]