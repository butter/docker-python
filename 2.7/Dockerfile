FROM python:2.7.13
MAINTAINER Butter.ai <dev@butter.ai>

# grab gosu for easy step-down from root
ENV GOSU_VERSION 1.10
RUN set -x \
 && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture)" \
 && wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture).asc" \
 && export GNUPGHOME="$(mktemp -d)" \
 && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
 && gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
 && rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc \
 && chmod +x /usr/local/bin/gosu \
 && gosu nobody true

RUN apt-get update && apt-get install -y --no-install-recommends \
    vim.tiny \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN groupadd -r butter && useradd -r -g butter butter

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
