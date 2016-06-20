FROM quay.io/stonevil/alpine-glibc
MAINTAINER stonevil@gmail.com

ENV DOCKERBEAT_VERSION=${DOCKERBEAT_VERSION:-1.0.0-rc1}

LABEL com.stonevil.dockerbeat.version=${DOCKERBEAT_VERSION}

RUN ${SOFTWARE_DOWNLOAD_CMD} -o /usr/bin/dockerbeat https://github.com/Ingensi/dockerbeat/releases/download/v${DOCKERBEAT_VERSION}/dockerbeat-${DOCKERBEAT_VERSION}-x86_64 && chmod +x /usr/bin/dockerbeat && \
  adduser -D -H -h /data/dockerbeat -s /bin/bash dockerbeat && \
  ${APK_CACHE_RM}

ADD _rootfs /
ONBUILD ADD _rootfs_vendor /

VOLUME ["/data/dockerbeat"]

ENV ELASTIC_LOGSTASH_HOSTS=${ELASTIC_LOGSTASH_HOSTS:-localhost:5044}

CMD mkdir -p /data/dockerbeat; chown -fR dockerbeat:dockerbeat /data/dockerbeat && \
  sed -i -e 's|_ELASTICLOGSTASHHOSTS_|'"${ELASTIC_LOGSTASH_HOSTS}"'|g' /etc/dockerbeat.yml && \
  gosu dockerbeat dockerbeat -configtest -c /etc/dockerbeat.yml -e && \
  gosu dockerbeat dockerbeat -c /etc/dockerbeat.yml -e
