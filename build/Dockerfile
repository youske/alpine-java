# Alpine Linux with java8
# frolvlad/alpine-oraclejdk8:slim

FROM frolvlad/alpine-oraclejdk8:slim
MAINTAINER youske miyakoshi <youske@gmail.com>

LABEL com.geeplus.description="alpine java image" \
      com.geeplus.role="baseimage"

ENV ENTRYKIT_VERSION=0.4.0
ENV ENTRYKIT_FILE=entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz
ENV ENTRYKIT_DOWNLOAD=https://github.com/progrium/entrykit/releases/download/v${ENTRYKIT_VERSION}/${ENTRYKIT_FILE}

ENV GOSU_VERSION=1.7
ENV GOSU_DOWNLOAD=https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-amd64

ENV PACKAGE='bash ca-certificates' \
    DOWNLOAD_PACKAGE='wget git' \
    LANG=C.UTF-8 \
    WORK_DIR=/home/admin

# apk install
RUN apk add --no-cache ${PACKAGE} && apk add --no-cache ${DOWNLOAD_PACKAGE} -t buildpack

# entrykit install
RUN wget -q --no-check-certificate ${ENTRYKIT_DOWNLOAD} -O- | tar zx && \
    chmod +x /entrykit && mv /entrykit /usr/bin && /usr/bin/entrykit --symlink

# gosu
RUN wget -q --no-check-certificate ${GOSU_DOWNLOAD} -O /usr/bin/gosu && chmod +x /usr/bin/gosu

RUN apk del buildpack --no-cache && rm -rf /tmp/* /var/cache/apk/*

# add admin user
RUN adduser -D -h /home/admin -s /bin/bash admin admin

WORKDIR ${WORK_DIR}
CMD ["/bin/bash"]
