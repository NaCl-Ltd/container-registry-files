FROM ruby:3.0.7-buster

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
## browser-test 実行に必要なパッケージ
RUN curl -SL https://deb.nodesource.com/setup_11.x | bash -
RUN apt-get install -y nodejs libasound2 libgtk-3-0 libnss3 libxss1 libatk-bridge2.0-0 fonts-liberation libpoppler-glib8
## xmlsectool 実行に必要なパッケージ
RUN echo "path-exclude /usr/lib/jvm/java-11-openjdk-amd64/man/*" > /etc/dpkg/dpkg.cfg.d/openjdk-11 && \
    apt update && \
    apt -y install --no-install-recommends openjdk-11-jre-headless
RUN echo "export JAVA_HOME=$(dirname $(dirname $(readlink $(readlink $(which java)))))" >> ~/.bashrc
RUN echo 'export PATH=$PATH:$JAVA_HOME/bin' >> ~/.bashrc
# 証明書が参照できずエラーとなる場合があるためシンボリックリンクを追加
RUN ln -s /etc/ssl/certs/ca-certificates.crt /usr/lib/ssl/cert.pem
## browser-testが文字化けしないようにするために必要なパッケージ
## 参考: https://www.yurikago-blog.com/posts/2
RUN apt-get install -y libpango1.0-0 \
    libxcomposite1 \
    libxcursor1 \
    libxdamage1 \
    libxext6 \
    libxi6 \
    libxtst6 \
    libcups2 \
    libxss1 \
    libxrandr2 \
    libgconf-2-4 \
    libasound2 \
    libatk1.0-0 \
    libgtk-3-0 \
    fonts-ipafont-gothic \
    xfonts-100dpi \
    xfonts-75dpi \
    x11-utils \
    xfonts-cyrillic \
    xfonts-base \
    libgbm1
CMD [ "irb" ]
