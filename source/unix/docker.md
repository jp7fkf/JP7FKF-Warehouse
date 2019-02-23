# Docker
 - 時代はコンテナ．

## はじめての
  ```
    yum install docker
    chkconfig docker on
    service docker start
  ```

## docker pull
  - docker hubからとってくる．
  - docker pull centos
  - docker pull nginx
  - とかとか．
  - `docker images`でとったやつを確認できる．

## 使い方
  - さがす
    - `docker search <image_name:tag>`
  - ひっぱる
    - `docker pull <image_name:tag>`
  - イメージ一覧
    - `docker image ls`
  - たてる
    - `docker run -it <image_name:tag> <command (ex./bin/bash)>`
      - iオプションは中に入る．tオプションはtty接続．
    - `docker run --name <container_name> -d <image_name:tag> <command> -p <host_port:container_port>`
      - `--name`: お名前つける
      - `-d`: daemonとして起動
      - `-p`: port forwardの設定．これでホストのポートにアクセスするとコンテナ内部のポートにフォワードされる．

    - 出るときは `Ctrl+p Ctrl+q`.exitするとコンテナが止まる．
  - コンテナ内に入る
    - `docker exec -it <id or name> <command>`
    - `exit`で抜けれる
  - プロセス一覧
    - `docker ps -a`
  - 削除
    - `docker rm <id or name>`
  - イメージを削除
    - `docker rmi -f <id or name>`
  - コンテナからイメージを作る
    - `docker commit <id> <yourimage_name:yourtag>`
  - attach
    - `docker attach <id or name>`
  - dettach
    - `Ctrl+p Ctrl+q`

## ホストとファイルコピー
```
  docker container ls <id or name>
  docker container cp <id or name>:/path/to/target_file /コピー先/ディレクトリ
```

## docker containerの中でcronしたい
  - コンテナに入って下記とか
  ```
    apt-get update
    apt-get install cron
    service cron start
    crontab -e
  ```

## もんだい
  - https://deeeet.com/writing/2015/02/17/docker-bad-points/