FROM centos:centos7

# The following is based on:
# https://github.com/docker-library/ruby/blob/fc6fd1e86d41ca7efa2737374ca12f73c3bc42ac/Dockerfile-slim.template
RUN yum install -y \
		bzip2 \
		ca-certificates \
		libffi-devel \
		gdbm \
		openssl-devel \
		libreadline \
		libyaml-devel \
		procps-ng \
		zlib-devel

# skip installing gem documentation
RUN mkdir -p /usr/local/etc \
	&& { \
		echo 'install: --no-document'; \
		echo 'update: --no-document'; \
	} >> /usr/local/etc/gemrc

ENV RUBY_MAJOR 3.0
ENV RUBY_VERSION 3.0.7
ENV RUBY_DOWNLOAD_SHA256 1748338373c4fad80129921080d904aca326e41bd9589b498aa5ee09fd575bab
ENV RUBYGEMS_VERSION 3.3.6
ENV BUNDLER_VERSION 2.3.6

# some of ruby's build scripts are written in ruby
#   we purge system ruby later to make sure our final image uses what we just built
RUN set -ex \
	\
	&& buildDeps=' \
		autoconf \
		bison \
		gcc \
		bzip2-devel \
		gdbm-devel \
		glib2-devel \
		ncurses-devel \
		libreadline-devel \
		libxml2-devel \
		libxslt-devel \
		make \
	' \
	&& yum install -y $buildDeps \
	&& curl --output ruby.tar.xz "https://cache.ruby-lang.org/pub/ruby/${RUBY_MAJOR%-rc}/ruby-$RUBY_VERSION.tar.xz" \
	&& echo "$RUBY_DOWNLOAD_SHA256 *ruby.tar.xz" | sha256sum -c - \
	\
	&& mkdir -p /usr/src/ruby \
	&& tar -xJf ruby.tar.xz -C /usr/src/ruby --strip-components=1 \
	&& rm ruby.tar.xz \
	\
	&& cd /usr/src/ruby \
	\
# hack in "ENABLE_PATH_CHECK" disabling to suppress:
#   warning: Insecure world writable dir
	&& { \
		echo '#define ENABLE_PATH_CHECK 0'; \
		echo; \
		cat file.c; \
	} > file.c.new \
	&& mv file.c.new file.c \
	\
	&& autoconf \
	&& gnuArch="$(uname -m)-linux-gnu" \
	&& ./configure \
		--build="$gnuArch" \
		--disable-install-doc \
		--enable-shared \
	&& make -j "$(nproc)" \
	&& make install \
	\
	&& yum erase -y $buildDeps \
	&& cd / \
	&& rm -r /usr/src/ruby \
	\
	&& gem update --system "$RUBYGEMS_VERSION" \
	&& gem install bundler --version "$BUNDLER_VERSION" --force

# install things globally, for great justice
# and don't create ".bundle" in all our apps
ENV GEM_HOME /usr/local/bundle
ENV BUNDLE_PATH="$GEM_HOME" \
	BUNDLE_SILENCE_ROOT_WARNING=1 \
	BUNDLE_APP_CONFIG="$GEM_HOME"
# path recommendation: https://github.com/bundler/bundler/pull/6469#issuecomment-383235438
ENV PATH $GEM_HOME/bin:$BUNDLE_PATH/gems/bin:$PATH
# adjust permissions of a few directories for running "gem install" as an arbitrary user
RUN mkdir -p "$GEM_HOME" && chmod 777 "$GEM_HOME"
# (BUNDLE_PATH = GEM_HOME, no need to mkdir/chown both)

RUN localedef -f UTF-8 -i ja_JP /usr/lib/locale/ja_JP.UTF-8
ENV LC_ALL=ja_JP.UTF-8
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# デフォルトで入っていないパッケージ
RUN yum install -y which sudo epel-release
RUN yum groupinstall -y 'Development Tools'

# sqlite3.gem のビルド時に必要なパッケージ
RUN yum install -y sqlite-devel

# Git
RUN yum install -y curl-devel expat-devel wget
RUN wget https://github.com/git/git/archive/refs/tags/v2.28.1.tar.gz \
  && tar xvzf v2.28.1.tar.gz \
  && cd git-2.28.1 \
  && make prefix=/usr/local all \
  && make prefix=/usr/local install \
  && cd / \
  && rm v2.28.1.tar.gz \
  && rm -r git-2.28.1

CMD [ "irb" ]
