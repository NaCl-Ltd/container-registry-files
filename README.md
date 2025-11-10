## 概要

[NaCl-Ltd の GitHub container registry](https://github.com/orgs/NaCl-Ltd/packages) のDockerイメージ生成に使用したDockerfileを管理するためのリポジトリ

## 重要

[NaCl-Ltd の GitHub container registry](https://github.com/orgs/NaCl-Ltd/packages) はPublicアクセスとなっています。プロジェクト固有のソースなどは含めいないよう注意!!

## How To

### 準備

```
docker login ghcr.io -u ${Githubアカウント名}
```

でログインできるようにしておいてください。また認証トークンには然るべきパーミッションが必要です。

### 新規イメージの作成

イメージ名を変数として定義
```
IMAGE_NAME=イメージ名
```

イメージ名ディレクトリ、Dockerfile を作成します。

```
mkdir ${IMAGE_NAME}

vi ${IMAGE_NAME}/Dockerfile
```

build して push します

```
cd ${IMAGE_NAME}

docker image build -t ${IMAGE_NAME}:latest .

docker tag ${IMAGE_NAME}:latest ghcr.io/nacl-ltd/${IMAGE_NAME}:latest

docker push ghcr.io/nacl-ltd/${IMAGE_NAME}:latest
```

### 別タグの付与

ここでは例として latest に別タグ ruby2.7 を付与する前提とする

```
IMAGE_NEW_TAG=ruby2.7

docker tag ${IMAGE_NAME}:latest ghcr.io/nacl-ltd/${IMAGE_NAME}:${IMAGE_NEW_TAG}

docker push ghcr.io/nacl-ltd/${IMAGE_NAME}:${IMAGE_NEW_TAG}
```
