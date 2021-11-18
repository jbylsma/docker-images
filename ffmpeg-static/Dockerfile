FROM centos:8

ENV FFMPEG_VERSION=4.4.1 \
    FDKAAC_VERSION=2.0.2 \
    FREETYPE_VERSION=2.11.0 \
    LAME_VERSION=3.100 \
    OPENSSL_VERSION=1.1.1l \
    OPUS_VERSION=1.3 \
    VPX_VERSION=1.11.0 \
    X264_VERSION=20191217-2245 \
    X265_VERSION=3.5

ARG FFMPEG_SHA256SUM=8fc9f20ac5ed95115a9e285647add0eedd5cc1a98a039ada14c132452f98ac42
ARG FDKAAC_SHA256SUM=7812b4f0cf66acda0d0fe4302545339517e702af7674dd04e5fe22a5ade16a90
ARG FREETYPE_SHA256SUM=a45c6b403413abd5706f3582f04c8339d26397c4304b78fa552f2215df64101f
ARG LAME_SHA256SUM=ddfe36cab873794038ae2c1210557ad34857a4b6bdc515785d1da9e175b1da1e
ARG OPENSSL_SHA256SUM=0b7a3e5e59c34827fe0c3a74b7ec8baef302b98fa80088d7f9153aa16fa76bd1
ARG OPUS_SHA256SUM=4f3d69aefdf2dbaf9825408e452a8a414ffc60494c70633560700398820dc550
ARG VPX_SHA1SUM=626ff35955c2c35b806b3e0ecf551a1a8611cdbf
ARG X264_SHA256SUM=0bb67d095513391e637b3b47e8efc3ba4603c3844f1b4c2690f4d36da7763055
ARG X265_SHA256SUM=e70a3335cacacbba0b3a20ec6fecd6783932288ebc8163ad74bcc9606477cae8

ARG PREFIX=/opt/ffmpeg
ARG BIN_DIR="${PREFIX}/bin"
ARG SRC_DIR="${PREFIX}/src"
ARG MAKEFLAGS="-j1"

RUN dnf -y install \
      autoconf \
      automake \
      bzip2 \
      cmake \
      diffutils \
      dnf-plugins-core \
      gcc \
      gcc-c++ \
      git \
      libtool \
      make \
      pkgconfig \
      which && \
    dnf config-manager --set-enabled powertools && \
    dnf -y install \
      glibc-static \
      libstdc++-static \
      nasm \
      yasm \
      zlib-static && \
    dnf clean all

# fdk-aac
RUN \
  DIR="${SRC_DIR}/fdk-aac" && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  curl -Lo source.tar.gz "https://github.com/mstorsjo/fdk-aac/archive/refs/tags/v${FDKAAC_VERSION}.tar.gz" && \
  echo "${FDKAAC_SHA256SUM} source.tar.gz" | sha256sum --check && \
  tar -zx --strip-components=1 -f source.tar.gz && \
  rm -f source.tar.gz && \
  autoreconf -fiv && \
  ./configure \
    --prefix="${PREFIX}" \
    --enable-shared=no \
    --datadir="${DIR}" && \
  make && \
  make install

# FreeType
RUN \
  DIR="${SRC_DIR}/freetype" && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  curl -Lo source.tar.gz "https://download.savannah.gnu.org/releases/freetype/freetype-${FREETYPE_VERSION}.tar.gz" && \
  echo "${FREETYPE_SHA256SUM} source.tar.gz" | sha256sum --check && \
  tar -zx --strip-components=1 -f source.tar.gz && \
  rm -f source.tar.gz && \
  ./configure \
    --prefix="${PREFIX}" \
    --disable-shared && \
  make && \
  make install

# Lame
RUN \
  DIR="${SRC_DIR}/lame" && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  curl -Lo source.tar.gz "https://sourceforge.net/projects/lame/files/lame/${LAME_VERSION}/lame-${LAME_VERSION}.tar.gz/download" && \
  echo "${LAME_SHA256SUM} source.tar.gz" | sha256sum --check && \
  tar -zx --strip-components=1 -f source.tar.gz && \
  rm -f source.tar.gz && \
  ./configure \
    --prefix="${PREFIX}" \
    --bindir="${BIN_DIR}" \
    --enable-shared=no \
    --enable-nasm \
    --disable-frontend && \
  make && \
  make install

# OpenSSL
RUN \
  DIR="${SRC_DIR}/openssl" && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  curl -Lo source.tar.gz "https://www.openssl.org/source/openssl-${OPENSSL_VERSION}.tar.gz" && \
  echo "${OPENSSL_SHA256SUM} source.tar.gz" | sha256sum --check && \
  tar -zx --strip-components=1 -f source.tar.gz && \
  rm -f source.tar.gz && \
  ./config \
    --prefix="${PREFIX}" \
    no-shared \
    no-tests && \
  make && \
  make install_sw

# Opus
RUN \
  DIR="${SRC_DIR}/opus" && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  curl -Lo source.tar.gz "https://archive.mozilla.org/pub/opus/opus-${OPUS_VERSION}.tar.gz" && \
  echo "${OPUS_SHA256SUM} source.tar.gz" | sha256sum --check && \
  tar -zx --strip-components=1 -f source.tar.gz && \
  rm -f source.tar.gz && \
  autoreconf -fiv && \
  ./configure \
    --prefix="${PREFIX}" \
    --enable-shared=no \
    --disable-doc \
    --disable-extra-programs && \
  make && \
  make install

# VPX
RUN \
  DIR="${SRC_DIR}/vpx" && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  git clone --branch "v${VPX_VERSION}" --depth 1 https://chromium.googlesource.com/webm/libvpx . && \
  test "$(git rev-parse --verify HEAD)" == "${VPX_SHA1SUM}" && \
  ./configure \
    --prefix="${PREFIX}" \
    --enable-pic \
    --disable-install-bins \
    --disable-examples \
    --disable-tools \
    --disable-docs \
    --enable-vp8 \
    --enable-vp9 \
    --enable-vp9-highbitdepth && \
  make && \
  make install

# x264
RUN \
  DIR="${SRC_DIR}/x264" && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  curl -Lo source.tar.bz2 "https://download.videolan.org/pub/videolan/x264/snapshots/x264-snapshot-${X264_VERSION}.tar.bz2" && \
  echo "${X264_SHA256SUM} source.tar.bz2" | sha256sum --check && \
  tar -jx --strip-components=1 -f source.tar.bz2 && \
  rm -f source.tar.bz2 && \
  ./configure \
    --prefix="${PREFIX}" \
    --disable-cli \
    --enable-static \
    --enable-pic && \
  make && \
  make install

# x265
RUN \
  DIR="${SRC_DIR}/x265" && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  curl -Lo source.tar.gz "https://bitbucket.org/multicoreware/x265_git/downloads/x265_${X265_VERSION}.tar.gz" && \
  echo "${X265_SHA256SUM} source.tar.gz" | sha256sum --check && \
  tar -zx --strip-components=1 -f source.tar.gz && \
  rm -f source.tar.gz && \
  cd build/linux && \
  cmake \
    -G "Unix Makefiles" \
    -DCMAKE_INSTALL_PREFIX="${PREFIX}" \
    -DENABLE_CLI:BOOL=OFF \
    -DENABLE_SHARED:BOOL=OFF \
    -DSTATIC_LINK_CRT:BOOL=ON \
    ../../source && \
  sed -i 's/-lgcc_s/-lgcc_eh/g' x265.pc && \
  make x265-static && \
  make install

# ffmpeg
RUN \
  DIR="${SRC_DIR}/ffmpeg" && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  curl -Lo source.tar.bz2 "https://ffmpeg.org/releases/ffmpeg-${FFMPEG_VERSION}.tar.bz2" && \
  echo "${FFMPEG_SHA256SUM} source.tar.bz2" | sha256sum --check && \
  tar -jx --strip-components=1 -f source.tar.bz2 && \
  rm -f source.tar.bz2 && \
  PATH="${BIN_DIR}:${PATH}" \
    PKG_CONFIG_PATH="${PREFIX}/lib/pkgconfig:$(pkg-config --variable pc_path pkg-config)" \
    ./configure \
      --prefix="${PREFIX}" \
      --pkg-config-flags="--static" \
      --extra-cflags="-I${PREFIX}/include" \
      --extra-ldflags="-L${PREFIX}/lib" \
      --extra-libs=-lpthread \
      --extra-libs=-lm \
      --extra-libs=-lz \
      --extra-ldexeflags="-static" \
      --bindir="${BIN_DIR}" \
      --enable-gpl \
      --enable-nonfree \
      --disable-ffplay \
      --disable-doc \
      --enable-libfdk_aac \
      --enable-libfreetype \
      --enable-libmp3lame \
      --enable-openssl \
      --enable-libopus \
      --enable-libx264 \
      --enable-libx265 \
      --disable-debug && \
  make && \
  make install

CMD bash
