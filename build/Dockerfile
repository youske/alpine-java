# Alpine Linux with java8
# frolvlad/alpine-oraclejdk8:slim

FROM frolvlad/alpine-oraclejdk8:slim
MAINTAINER youske miyakoshi <youske@gmail.com>

LABEL com.geeplus.description="alpine oraclejdk8 image" \
      com.geeplus.vendor="geeplus.inc" \
      com.geeplus.role="baseimage"

ENV ENTRYKIT_VERSION=0.4.0
ENV ENTRYKIT_FILE=entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz
ENV ENTRYKIT_DOWNLOAD=https://github.com/progrium/entrykit/releases/download/v${ENTRYKIT_VERSION}/${ENTRYKIT_FILE}

ENV PACKAGE='bash libstdc++' \
    DOWNLOAD_PACKAGE='wget git ca-certificates ' \
    LANG=C.UTF-8 \
    WORK_DIR=/home/admin

# apk install
RUN apk add --no-cache ${PACKAGE} && apk add --no-cache ${DOWNLOAD_PACKAGE} -t dlpack

# entrykit install
RUN wget -q --no-check-certificate ${ENTRYKIT_DOWNLOAD} -O- | tar zx && \
    chmod +x /entrykit && mv /entrykit /bin && /bin/entrykit --symlink

RUN apk del dlpack --no-cache && rm -rf /tmp/* /var/cache/apk/*

# add admin user
RUN adduser -D -h /home/admin -s /bin/bash admin admin

WORKDIR ${WORK_DIR}
CMD ["/bin/bash"]
