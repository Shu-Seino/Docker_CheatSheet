# Docker_CheatSheet
# dockerfileから環境構築

## Dockerfileの構成

```docker
# ubuntu:18.04 の Docker イメージからレイヤを作成
FROM ubuntu:18.04

# Dockerクライアントで操作しているディレクトリから、ファイルを（コンテナのレイヤに）追加
COPY . /app

# RUN はアプリケーションをmakeで構築/Dockerfileからimageを作成する時に実行
RUN make /app

# CMD はコンテナ内で何のコマンドを実行するか指定/DockerイメーシからDockerコンテナを作成される時に実行
CMD python /app/app.py

# bashを使う方法
SHELL ["/bin/bash", "-c"]
```

## Dockerfileを使用してDockerイメージをビルド

```bash
docker image build -t {イメージ名:タグ名} {Dockerfileのディレクトリ}

docker image build -t test-image:v1 .
```

- `t`オプションを使用すると、イメージ名(REPOSITORY)とタグ(TAG)を指定できます。
- `t`オプションを使用しない場合、イメージ名(REPOSITORY)とタグ(TAG)の値は`<none>`になります。

## Dockerイメージの一覧

```bash
docker image ls # docker imageの一覧を表示
REPOSITORY        TAG      IMAGE ID       CREATED             SIZE
<none>            <none>   a086e6cb87f0   40 seconds ago      800MB
test-image        v1       806faed5d0a8   About an hour ago   800MB
```

## Dockerイメージを使って、作成実行

```bash

# docker container run [options] イメージ名:[タグ名]
docker container run --name {コンテナ名} -it {image名} /bin/bash

docker container run --name test-container1 -it test-image:v1 /bin/bash

root@71fdfe8dfb61:/# cat /etc/issue
Ubuntu 18.04.6 LTS \n \l
```
## コンテナの作成

```bash
# -pを使用してポートを開くことも可能　EX）　ginでは8080ポートを使用するので　XXXX:8080(gin)
docker container create　　-p 8080:8080  --name {コンテナ名} -it {image名} 
```


## コンテナの起動

```bash
docker container start {CONTAINER IDまたはコンテナ名}
```

## コンテナのbash、shに入る

```bash
docker exec -it {CONTAINER IDまたはコンテナ名} /bin/bash
docker exec -it {CONTAINER IDまたはコンテナ名} /bin/sh
```

## コンテナに入る

```bash
$ docker attach {CONTAINER IDまたはコンテナ名}
```

## コンテナの一覧

```bash
# 実行中のコンテナのみ
docker ps
# 全てのコンテナ
docker ps -a
```

## コンテナを削除

```bash
docker rm -f {CONTAINER ID or コンテナ名}
# 停止中全てのコンテナ全削除
docker rm $(docker ps -q -a)  
```

## Dockerイメージの削除

```bash
docker rmi {IMAGE ID or イメージ名}
# 使用していないイメージ前削除
docker rmi $(docker images -q)
```
