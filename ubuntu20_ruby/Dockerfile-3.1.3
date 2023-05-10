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
WORKDIR /usr/local
RUN git clone https://github.com/sstephenson/rbenv.git rbenv
RUN git clone https://github.com/sstephenson/ruby-build.git rbenv/plugins/ruby-build
RUN echo "export RBENV_ROOT=\"/usr/local/rbenv\"\n\
export PATH=\"${RBENV_ROOT}/bin:${PATH}\"\n\
export CONFIGURE_OPTS=--disable-install-doc\n\
eval \"\$(/usr/local/rbenv/bin/rbenv init --no-rehash -)\"" >> /usr/local/.bash_profile
RUN . /usr/local/.bash_profile; rbenv install 3.1.3
RUN . /usr/local/.bash_profile; rbenv global 3.1.3
RUN . /usr/local/.bash_profile; gem i bundler --no-document -v 2.3.18
