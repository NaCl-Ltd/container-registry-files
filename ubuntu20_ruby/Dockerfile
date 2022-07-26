FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive
RUN apt update && apt install -y \
  apt-utils \
  build-essential \
  debhelper \
  devscripts \
  dh-make \
  dpkg-dev \
  gdebi \
  git \
  nodejs \
  libffi-dev \
  libpq-dev \
  libreadline-dev \
  libssl-dev \
  systemd \
  sudo \
  tzdata \
  wget \
  zlib1g-dev \
  language-pack-ja

ENV TZ='Asia/Tokyo'
RUN locale-gen ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8
RUN rbenv install 3.1.2
RUN rbenv global 3.1.2
RUN gem i bundler --no-document -v 2.3.18
