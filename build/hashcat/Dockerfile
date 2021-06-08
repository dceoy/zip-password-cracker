FROM nvidia/cuda:11.3.0-runtime-ubuntu20.04

ADD https://raw.githubusercontent.com/dceoy/print-github-tags/master/print-github-tags /usr/local/bin/print-github-tags

RUN set -e \
      && ln -sf /bin/bash /bin/sh

RUN set -e \
      && apt-get -y update \
      && apt-get -y dist-upgrade \
      && apt-get -y install --no-install-recommends --no-install-suggests \
        ca-certificates curl p7zip-full \
      && apt-get -y autoremove \
      && apt-get clean \
      && rm -rf /var/lib/apt/lists/*

RUN set -eo pipefail \
      && chmod +x /usr/local/bin/print-github-tags \
      && print-github-tags --release --latest hashcat/hashcat \
        | tr -d v \
        | xargs -i curl -SL \
          https://github.com/hashcat/hashcat/releases/download/v{}/hashcat-{}.7z \
          -o /tmp/hashcat.7z \
      && 7z x -o/opt/ /tmp/hashcat.7z \
      && mv /opt/hashcat-* /opt/hashcat \
      && rm -f /tmp/hashcat.7z

WORKDIR /opt/hashcat

ENTRYPOINT ["/opt/hashcat/hashcat.bin"]
