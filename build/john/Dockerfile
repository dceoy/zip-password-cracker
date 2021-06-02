FROM ubuntu:latest

ENV DEBIAN_FRONTEND noninteractive

ADD https://raw.githubusercontent.com/dceoy/print-github-tags/master/print-github-tags /usr/local/bin/print-github-tags

RUN set -e \
      && ln -sf bash /bin/sh

RUN set -e \
      && apt-get -y update \
      && apt-get -y dist-upgrade \
      && apt-get -y install --no-install-recommends --no-install-suggests \
        build-essential ca-certificates curl git libssl-dev zlib1g-dev \
      && apt-get -y autoremove \
      && apt-get clean \
      && rm -rf /var/lib/apt/lists/*

RUN set -eo pipefail \
      && chmod +x /usr/local/bin/print-github-tags \
      && git clone --depth 1 \
        https://github.com/openwall/john /opt/john \
      && cd /opt/john/src \
      && ./configure \
      && make clean \
      && make \
      && make install

ENV PATH /opt/john/run:${PATH}

ENTRYPOINT ["/opt/john/run/john"]
