FROM ruby:3.4.7

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
RUN apt-get update && apt-get install -y sudo poppler-utils build-essential
## mysql、mysqldump コマンドを使用するため、mariadb-server パッケージをインストール
RUN apt-get install -y libmariadb-dev mariadb-server
# JavaScriptの実行用のパッケージをインストール
RUN apt-get install -y libv8-dev nodejs

# Git
RUN apt-get update && apt-get install -y git
