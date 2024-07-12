FROM ruby:3.0.7

RUN apt-get update
RUN apt-get install -y locales
RUN localedef -f UTF-8 -i ja_JP /usr/lib/locale/ja_JP.UTF-8
ENV LC_ALL=ja_JP.UTF-8
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

RUN apt-get update && apt-get install -y \
		bzip2 \
		ca-certificates \
		libffi-dev \
		libgdbm-dev \
		libssl-dev \
		libreadline-dev \
		libyaml-dev \
		procps \
		zlib1g-dev

## デフォルトで入っていないパッケージ
RUN apt-get update && apt-get install -y sudo software-properties-common
RUN apt-get install -y build-essential
## mysql、mysqldump コマンドを使用するため、mariadb-server パッケージをインストール
RUN apt-get install -y libmariadb-dev mariadb-server sudo 
# JavaScriptの実行用のパッケージをインストール
RUN apt-get install -y libv8-dev nodejs

# Git
RUN apt-get update && apt-get install -y libcurl4-gnutls-dev libexpat1-dev wget gettext
RUN wget https://github.com/git/git/archive/refs/tags/v2.28.1.tar.gz \
  && tar xvzf v2.28.1.tar.gz \
  && cd git-2.28.1 \
  && make prefix=/usr/local all \
  && make prefix=/usr/local install \
  && cd / \
  && rm v2.28.1.tar.gz \
  && rm -r git-2.28.1
