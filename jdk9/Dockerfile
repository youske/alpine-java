# Alpine Linux with oraclejdk9

FROM frolvlad/alpine-oraclejdk8:slim
MAINTAINER youske miyakoshi <youske@gmail.com>

LABEL com.geeplus.description="alpine oraclejdk9 image" \
      com.geeplus.role="baseimage"

# ----- common

# s6-overlay
ENV S6OVERLAY_VERSION="v1.11.0.1"
ENV S6OVERLAY_URL="https://github.com/just-containers/s6-overlay/releases/download/${S6OVERLAY_VERSION}/s6-overlay-amd64.tar.gz"
ADD ${S6OVERLAY_URL} /tmp/

ENV ENTRYKIT_VERSION=0.4.0
ENV ENTRYKIT_FILE=entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz
ENV ENTRYKIT_DOWNLOAD=https://github.com/progrium/entrykit/releases/download/v${ENTRYKIT_VERSION}/${ENTRYKIT_FILE}

ENV GOSU_VERSION=1.7
ENV GOSU_DOWNLOAD=https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-amd64

ENV PACKAGE='bash wget ca-certificates' \
    BUILD_PACKAGE='git build-base musl-dev linux-headers' \
    LANG=C.UTF-8 \
    WORK_DIR=/home/admin

# add admin user
RUN adduser -D -h /home/admin -s /bin/bash admin admin

# apk install
RUN apk upgrade --no-cache && \
    apk add --no-cache ${PACKAGE} && \
    apk add --no-cache ${BUILD_PACKAGE} -t buildpack

# entrykit install
RUN wget -q --no-check-certificate ${ENTRYKIT_DOWNLOAD} -O- | tar zx && \
    chmod +x /entrykit && mv /entrykit /usr/bin && /usr/bin/entrykit --symlink

# gosu
RUN wget -q --no-check-certificate ${GOSU_DOWNLOAD} -O /usr/bin/gosu && chmod +x /usr/bin/gosu

# s6-overlay
RUN tar zxf /tmp/s6-overlay-amd64.tar.gz -C /

RUN mkdir -p /workdir

RUN apk del buildpack --no-cache && rm -rf /tmp/* /var/cache/apk/*

WORKDIR ${WORK_DIR}
ENTRYPOINT ["switch","shell=su admin -c /bin/bash","--","switch","root=/bin/bash","--","/init"]
CMD ["su","admin","-c","/bin/bash"]
