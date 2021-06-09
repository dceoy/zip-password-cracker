FROM nvidia/cuda:11.3.1-base-ubuntu20.04

ADD https://raw.githubusercontent.com/dceoy/print-github-tags/master/print-github-tags /usr/local/bin/print-github-tags

RUN set -e \
      && ln -sf /bin/bash /bin/sh

RUN set -e \
      && apt-get -y update \
      && apt-get -y dist-upgrade \
      && apt-get -y install --no-install-recommends --no-install-suggests \
        ca-certificates clinfo curl g++ gcc libc6-dev libcurl4-gnutls-dev \
        libssl-dev make ocl-icd-libopencl1 pkg-config zlib1g-dev \
      && apt-get -y autoremove \
      && apt-get clean \
      && rm -rf /var/lib/apt/lists/*

RUN set -e \
      && mkdir -p /etc/OpenCL/vendors \
      && echo 'libnvidia-opencl.so.1' > /etc/OpenCL/vendors/nvidia.icd \
      && echo '/usr/local/nvidia/lib' >> /etc/ld.so.conf.d/nvidia.conf \
      && echo '/usr/local/nvidia/lib64' >> /etc/ld.so.conf.d/nvidia.conf

ENV PATH /usr/local/nvidia/bin:${PATH}
ENV LD_LIBRARY_PATH /usr/local/nvidia/lib:/usr/local/nvidia/lib64:${LD_LIBRARY_PATH}
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility

RUN set -eo pipefail \
      && chmod +x /usr/local/bin/print-github-tags \
      && print-github-tags --release --latest --tar hashcat/hashcat \
        | xargs -i curl -SL {} -o /tmp/hashcat.tar.gz \
      && tar xvf /tmp/hashcat.tar.gz -C /usr/local/src --remove-files \
      && mv /usr/local/src/hashcat-* /usr/local/src/hashcat \
      && cd /usr/local/src/hashcat \
      && make install \
      && rm -f /tmp/hashcat.tar.gz

RUN set -eo pipefail \
      && print-github-tags --release --latest --tar hashcat/hashcat-utils \
        | xargs -i curl -SL {} -o /tmp/hashcat-utils.tar.gz \
      && tar xvf /tmp/hashcat-utils.tar.gz -C /usr/local/src --remove-files \
      && mv /usr/local/src/hashcat-utils-* /usr/local/src/hashcat-utils \
      && cd /usr/local/src/hashcat-utils/src \
      && make \
      && find /usr/local/src/hashcat-utils/src -type f -executable \
        -exec ln -s {} /usr/local/bin \; \
      && rm -f /tmp/hashcat.tar.gz

RUN set -eo pipefail \
      && chmod +x /usr/local/bin/print-github-tags \
      && print-github-tags --release --latest --tar ZerBea/hcxtools \
        | xargs -i curl -SL {} -o /tmp/hcxtools.tar.gz \
      && tar xvf /tmp/hcxtools.tar.gz -C /usr/local/src --remove-files \
      && mv /usr/local/src/hcxtools-* /usr/local/src/hcxtools \
      && cd /usr/local/src/hcxtools \
      && make install \
      && rm -f /tmp/hcxtools.tar.gz

RUN set -eo pipefail \
      && chmod +x /usr/local/bin/print-github-tags \
      && print-github-tags --release --latest --tar ZerBea/hcxdumptool \
        | xargs -i curl -SL {} -o /tmp/hcxdumptool.tar.gz \
      && tar xvf /tmp/hcxdumptool.tar.gz -C /usr/local/src --remove-files \
      && mv /usr/local/src/hcxdumptool-* /usr/local/src/hcxdumptool \
      && cd /usr/local/src/hcxdumptool \
      && make install \
      && rm -f /tmp/hcxdumptool.tar.gz

RUN set -eo pipefail \
      && print-github-tags --release --latest --tar hashcat/kwprocessor \
        | xargs -i curl -SL {} -o /tmp/kwprocessor.tar.gz \
      && tar xvf /tmp/kwprocessor.tar.gz -C /usr/local/src --remove-files \
      && mv /usr/local/src/kwprocessor-* /usr/local/src/kwprocessor \
      && cd /usr/local/src/kwprocessor \
      && make \
      && find /usr/local/src/kwprocessor -type f -executable \
        -exec ln -s {} /usr/local/bin \; \
      && rm -f /tmp/hashcat.tar.gz

ENTRYPOINT ["/usr/local/bin/hashcat"]
