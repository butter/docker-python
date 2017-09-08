FROM debian:jessie
LABEL maintainer="Butter.ai <dev@butter.ai>"

ENV \
  PATH=/usr/local/bin:$PATH \
  LANG=C.UTF-8 \
  GOSU_VERSION=1.10 \
  PYTHON_VERSION=3.6.2 \
  PYTHON_PIP_VERSION=9.0.1

# Install python
RUN set -ex; \
    buildDeps='dpkg-dev tcl-dev tk-dev' \
 && apt-get update && apt-get install -y --no-install-recommends \
    bzip2 \
    ca-certificates \
    curl \
    dirmngr \
    gcc \
    g++ \
    gnupg2 \
    libc6-dev \
    libcurl4-openssl-dev \
    libglib2.0-dev \
    libssl-dev \
    libxml2-dev \
    make \
    patch \
    wget \
    xz-utils \
    zlib1g-dev \
    $buildDeps \
 && rm -rf /var/lib/apt/lists/* \
 \
 && wget -O python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz" \
 && wget -O python.tar.xz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc" \
 && export GNUPGHOME="$(mktemp -d)" \
 && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "0D96DF4D4110E5C43FBFB17F2D347EA6AA65421D" \
 && gpg --batch --verify python.tar.xz.asc python.tar.xz \
 && rm -rf "$GNUPGHOME" python.tar.xz.asc \
 && mkdir -p /usr/src/python \
 && tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz \
 && rm python.tar.xz \
 \
 && cd /usr/src/python \
 && gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
 && ./configure \
   --build="$gnuArch" \
   --enable-loadable-sqlite-extensions \
   --enable-shared --with-system-expat \
   --with-system-ffi \
   --without-ensurepip \
 && make -j "$(nproc)" \
 && make install \
 && ldconfig \
 \
 && apt-get purge -y --auto-remove $buildDeps \
 \
 && find /usr/local -depth \
   \( \
     \( -type d -a \( -name test -o -name tests \) \) \
     -o \
     \( -type f -a \( -name '*.pyc' -o -name '*.pyo' \) \) \
   \) -exec rm -rf '{}' + \
 && rm -rf /usr/src/python

# make some useful symlinks that are expected to exist
RUN cd /usr/local/bin \
	&& ln -s idle3 idle \
	&& ln -s pydoc3 pydoc \
	&& ln -s python3 python \
	&& ln -s python3-config python-config

# Install python pip
RUN set -x; \
    wget -O get-pip.py 'https://bootstrap.pypa.io/get-pip.py' \
 \
 && python get-pip.py --disable-pip-version-check --no-cache-dir \
    pip==$PYTHON_PIP_VERSION \
 && pip --version \
 \
 && find /usr/local -depth \
   \( \
     \( -type d -a \( -name test -o -name tests \) \) \
     -o \
     \( -type f -a \( -name '*.pyc' -o -name '*.pyo' \) \) \
   \) -exec rm -rf '{}' + \
 && rm -f get-pip.py

# grab gosu for easy step-down from root
RUN set -x \
 && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture)" \
 && wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture).asc" \
 && export GNUPGHOME="$(mktemp -d)" \
 && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
 && gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
 && rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc \
 && chmod +x /usr/local/bin/gosu \
 && gosu nobody true

RUN groupadd -r -g 999 butter \
 && useradd -r -u 999 -g 999 butter \
 && mkdir -p /usr/src/app /var/log/butter \
 && chown butter:butter /usr/src/app /var/log/butter

VOLUME /var/log/butter

WORKDIR /usr/src/app

CMD ["python3"]
