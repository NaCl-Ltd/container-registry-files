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
ENV RUBYGEMS_VERSION 3.2.32
ENV BUNDLER_VERSION 2.2.32

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
# mysql、mysqldump コマンドを使用するため、mariadb-server パッケージをインストール
RUN yum install -y mariadb-devel mariadb-server sudo
# browser-test 実行に必要なパッケージ
RUN yum install -y https://rpm.nodesource.com/pub_11.x/el/7/x86_64/nodesource-release-el7-1.noarch.rpm
RUN yum install -y nodejs alsa-lib gtk3 nss libXScrnSaver at-spi2-atk liberation-mono-fonts liberation-narrow-fonts liberation-sans-fonts  liberation-serif-font poppler-utils
# xmlsectool 実行に必要なパッケージ
RUN yum install -y  java-1.8.0-openjdk
RUN echo "export JAVA_HOME=$(dirname $(dirname $(readlink $(readlink $(which java)))))" > /etc/profile.d/java.sh
RUN echo 'export PATH=$PATH:$JAVA_HOME/bin' >> /etc/profile.d/java.sh
# browser-testが文字化けしないようにするために必要なパッケージ
# 参考: https://www.yurikago-blog.com/posts/2
RUN yum install -y pango.x86_64 \
    libXcomposite.x86_64 \
    libXcursor.x86_64 \
    libXdamage.x86_64 \
    libXext.x86_64 \
    libXi.x86_64 \
    libXtst.x86_64 \
    cups-libs.x86_64 \
    libXScrnSaver.x86_64 \
    libXrandr.x86_64 \
    GConf2.x86_64 \
    alsa-lib.x86_64 \
    atk.x86_64 \
    gtk3.x86_64 \
    ipa-gothic-fonts \
    xorg-x11-fonts-100dpi \
    xorg-x11-fonts-75dpi \
    xorg-x11-utils \
    xorg-x11-fonts-cyrillic \
    xorg-x11-fonts-Type1 \
    xorg-x11-fonts-misc

CMD [ "irb" ]
