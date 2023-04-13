## 概要

[NaCl-Ltd の GitHub container registry](https://github.com/orgs/NaCl-Ltd/packages) のDockerイメージ生成に使用したDockerfileを管理するためのリポジトリ

## 前提

```
docker login ghcr.io -u ${Githubアカウント名}
```

でログインできるようにしておいてください。また認証トークンには然るべきパーミッションが必要です。

## 新規イメージの作成

イメージ名ディレクトリ、Dockerfile を作成します。

```
mkdir ${イメージ名}

vi ${イメージ名}/Dockerfile
```

build して push します

```
docker image build -t ${イメージ名}:latest ${イメージ名}

docker tag ${イメージ名}:latest ghcr.io/nacl-ltd/${イメージ名}:latest

docker push ghcr.io/nacl-ltd/${イメージ名}:latest
```

## 別バージョンの作成

別バージョン(例:ruby2.7)用の Dockerfile を作成

```
vi ${イメージ名}/Dockerfile-ruby2.7
```

build して push します

```
docker image build -t ${イメージ名}:ruby2.7 -f Dockerfile-ruby2.7 ${イメージ名}

docker tag ${イメージ名}:ruby2.7 ghcr.io/nacl-ltd/${イメージ名}:ruby2.7

docker push ghcr.io/nacl-ltd/${イメージ名}:ruby2.7
```
