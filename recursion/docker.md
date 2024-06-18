# キャッシュのクリア
Dockerを使用してアプリケーションをビルドおよび実行する際に、問題が発生することがある場合、キャッシュや過去のイメージをクリアすることが助けになることがあります。具体的には、以下のようなケースが考えられます：

1. キャッシュのクリア
Dockerはビルド時にキャッシュを使用して効率的にイメージを構築しますが、時にはキャッシュが問題の原因になることがあります。例えば、パッケージや依存関係が更新された場合、古いキャッシュが残っていると正しくビルドされないことがあります。

キャッシュのクリア方法:
```bash
% docker-compose down && docker-compose build --no-cache && docker-compose up
docker-compose build --no-cache: 
```
--no-cache オプションを使用することで、キャッシュを無視して新たにビルドを行います。
2. 不要なイメージの削除
過去にビルドしたイメージや、使用していないイメージが残っている場合もあります。これらが大きなディスク使用量を引き起こしたり、イメージの選択を誤ってしまうことがあります。

不要なイメージの削除方法:
```bash
% docker image prune
```
docker image prune: 使用されていないすべてのイメージを削除します。不要なイメージをクリアすることで、ディスクの使用量を減らし、問題の原因を特定することができます。
3. ボリュームの削除
Dockerコンテナが使用していたボリュームが残っている場合、これも問題の原因になることがあります。特に、ボリュームが変更されている可能性がある場合は、クリアしておくと安全です。

ボリュームの削除方法:
```bash
% docker volume prune
```
docker volume prune: 使用されていないすべてのボリュームを削除します。ボリュームはデータを永続化するために使用されるため、特に開発環境で多くのボリュームが生成されることがあります。
- 結論
問題が発生している場合や、更新を反映させたい場合は、キャッシュや不要なイメージ、ボリュームをクリアしてからビルドや実行を行うことが推奨されます。特に開発環境では、頻繁にこの手順を実行して環境をクリーンに保つことが良い習慣です。
----

docker image が container を作るコードで、container どうし(例えば app と DB それぞれの container)を一緒に networking して起動できるのが docker compose の yml ファイルという考えで合ってる?

ChatGPT

はい、その考えは正しいです

docker run -d -p 30:30 とかをして container を起動させなくても、docker compose up をすれば 2 つの container が自動的に立ち上がるの?

はい、その通りです

[GitHub - docker/getting-started at 6190776cb618b1eb3cfb21e207eefde511d13449](https://github.com/docker/getting-started/tree/6190776cb618b1eb3cfb21e207eefde511d13449)

## スタート方法

```bash
docker run -d -p 80:80 docker/getting-started
これをした後にlocalhostでサイトが表示される

//build a container image(build image with Dockerfile)
//dockerをbuildするときに"getting-started"(image名)はタグとして自由に命名すことができる。
docker build -t getting-started

//then run app from image(make new instance)
docker run -dp 3000:3000 getting-started
```

image を build(Dockerfile)してそこから application 用の container を build する

## docker 環境で動いているコード修正を反映させたかったら

今動いている container を停止させる → その停止した container を削除する

```bash
////containerを停止させる////

//get containerID to which one you want to stop
% docker ps

% docker stop containerID

% docker rm containerID

//更新したコードをdockerで再び動かす
docker run -dp 3000:3000 getting-started

これでコードがブラウザでも更新された。
```

## Docker image を share(個人開発でも、本番環境へ移行するときにデプロイ用のサーバーへ共有する必要がある)

Docker Hub へ push する

```bash

//Docker hubへログイン
docker login -u kazumawada34

//imageにtagをつける
docker tag getting-started kazumawada34/getting-started

//push
docker push kazumawada34/getting-started

これでDockerhubにpushされた
```

# DB container 消した後のデータの保持 Volumes

そもそも、image から作られた container は create,update,delete をしたりしても、一旦削除してもう一度 image から作られる頃にはそのデータも消えてリセットされてしまうようになっている。

そこで出てくるのが Volumes

## named volumes(= bucket of data)

これで保存すれば、container でも以前の変更データを保持できる

```bash
% docker volume create todo-db

//todo-dbというvolumeを作った後、そのvolumeを使用してcontainerを起動する
//todosという-volumeを/etc/todosにmountするよという意味
% docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started

//////////////////////////////////////////////////////////////////////
どこに保存されているのか。
mountpointを見てみると、ローカルのディスクにデータが保存されていることがわかる。
% docker volume inspect todo-db
[
    {
        "CreatedAt": "2024-05-20T03:58:06Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
        "Name": "todo-db",
        "Options": null,
        "Scope": "local"
    }
]
```

# Multi-Container apps

server と DB2 つそれぞれに docker が必要だよねという話。

docker は、each container should do one thing and do it well というコンセプトがあるから別々に作る

app と DB それぞれ networking をして対話をできるようにする必要がある。

やり方は、最初につなげる設定をするのと、後からつなげる方法がある。

最初からつなげる方法

```bash
//create network
% docker network create todo-app

/////////////////////////////////////////////
//start MySQL container
//you can skip "volume create", just simply add "-v todo-mysql-data:/var/lib/mysql" then this guy automatically make volume.
% docker run -d \
    --network todo-app --network-alias mysql \
    -v todo-mysql-data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_DATABASE=todos \
    mysql:8.0

 //DB実行(いつものパスワード聞かれるやつ)
 docker exec -it <mysql-container-id> mysql -p

 //todosが登録されている
 mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| todos              |
+--------------------+
5 rows in set (0.03 sec)

//networkingしていく前に、
//公式がnetwrkingをするのに、nicolaka/netshootというcontainerを使っている。
% docker run -it --network todo-app nicolaka/netshoot
%dig mysql//これでipアドレスを確認できる
/////////////////////////////////////////////

//networkingしていく
% docker run -dp 3000:3000 \
  -w /app -v "$(pwd):/app" \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node:18-alpine \
  sh -c "yarn install && yarn run dev"

  上記のコマンドで実行すると、todo appで記入したリストがdatabaseに格納されている

```

# docker compose

- multi-container を share するときに使用する。
- YAML ファイルを使用する
- 誰でも compose up すれば環境が整う

app と DB を組み合わせるとこんな感じ

このファイルを

docker compose up -d

で起動させる

docker compose down で終了させる

```docker
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```
