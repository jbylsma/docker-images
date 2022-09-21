FROM debian:bullseye-slim

ENV LIBJPEG_TURBO_VERSION=2.1.4

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    make \
    cmake \
    yasm \
    gcc \
    gcc-multilib \
    libavutil-dev \
    libswscale-dev \
    libasound2-dev \
    libspeex-dev \
    libusbmuxd-dev \
    libplist-dev \
    gtk+-3.0 \
    libayatana-appindicator3-dev \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /opt/libjpeg-turbo && \
    curl -L "https://github.com/libjpeg-turbo/libjpeg-turbo/archive/refs/tags/${LIBJPEG_TURBO_VERSION}.tar.gz" | \
    tar -xzv -C /opt/libjpeg-turbo --strip-components=1 && \
    cd /opt/libjpeg-turbo && \
    cmake -G"Unix Makefiles" ./ && \
    make install

RUN bash
