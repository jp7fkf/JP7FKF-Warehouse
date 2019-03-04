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
  - `docker pull centos`
  - `docker pull nginx`
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

## tagをとりたい
  - いやこれふつうにやりたくなるんだけどコマンドになってないのだろうか？
  ```
    % curl -s -S 'https://registry.hub.docker.com/v2/repositories/library/centos/tags/' | jq '."results"[]["name"]' |sort
    "6"
    "6.6"
    "6.7"
    "7"
    "7.6.1810"
    "centos6"
    "centos6.6"
    "centos7"
    "centos7.6.1810"
    "latest"
```

## コンテナの詳細なデータを見たい 
  - `docker inspect <namme/id>`
  - たとえばIPがほしいときは
    - `docker inspect --format '{{ .NetworkSettings.IPAddress }}' <id/name>`

## docker runのときにport forwardingするの忘れたとき．
  - iptablesに書いてしまおう
    - `sudo iptables -t nat -A DOCKER ! -i docker0 -p tcp -m tcp --dport 8001 -j DNAT --to-destination 172.17.0.2:8888`
    - `sudo iptables -A DOCKER -d 172.17.0.2/32 ! -i docker0 -o docker0 -p tcp -m tcp --dport 8001 -j ACCEPT`
    - http://sagantaf.hatenablog.com/entry/2018/11/18/155131

## もんだい
  - https://deeeet.com/writing/2015/02/17/docker-bad-points/

## 脳死でcentosにdocker入れる
 - もう入ってた．うける．

  ```
    [jp7fkf@jp7fkf_0 ~]$ sudo yum install -y yum-utils \
    >   device-mapper-persistent-data \
    >   lvm2
    Loaded plugins: fastestmirror, langpacks
    Loading mirror speeds from cached hostfile
    epel/x86_64/metalink                                                                           | 9.8 kB  00:00:00
     * base: ftp.nara.wide.ad.jp
     * epel: ftp.riken.jp
     * extras: ftp.nara.wide.ad.jp
     * remi-safe: ftp.riken.jp
     * updates: ftp.nara.wide.ad.jp
    base                                                                                           | 3.6 kB  00:00:00
    epel                                                                                           | 4.7 kB  00:00:00
    extras                                                                                         | 3.4 kB  00:00:00
    remi-safe                                                                                      | 3.0 kB  00:00:00
    updates                                                                                        | 3.4 kB  00:00:00
    Package yum-utils-1.1.31-50.el7.noarch already installed and latest version
    Package device-mapper-persistent-data-0.7.3-3.el7.x86_64 already installed and latest version
    Package 7:lvm2-2.02.180-10.el7_6.3.x86_64 already installed and latest version
    Nothing to do
    [jp7fkf@jp7fkf_0 ~]$ sudo yum-config-manager \
    >     --add-repo \
    >     https://download.docker.com/linux/centos/docker-ce.repo
    Loaded plugins: fastestmirror, langpacks
    adding repo from: https://download.docker.com/linux/centos/docker-ce.repo
    grabbing file https://download.docker.com/linux/centos/docker-ce.repo to /etc/yum.repos.d/docker-ce.repo
    repo saved to /etc/yum.repos.d/docker-ce.repo
    [jp7fkf@jp7fkf_0 ~]$
    [jp7fkf@jp7fkf_0 ~]$ sudo yum install docker-ce docker-ce-cli containerd.io
    Loaded plugins: fastestmirror, langpacks
    Loading mirror speeds from cached hostfile
     * base: ftp.nara.wide.ad.jp
     * epel: mirrors.aliyun.com
     * extras: ftp.nara.wide.ad.jp
     * remi-safe: ftp.riken.jp
     * updates: ftp.nara.wide.ad.jp
    docker-ce-stable                                                                               | 3.5 kB  00:00:00
    (1/2): docker-ce-stable/x86_64/primary_db                                                      |  23 kB  00:00:00
    (2/2): docker-ce-stable/x86_64/updateinfo                                                      |   55 B  00:00:00
    Resolving Dependencies
    --> Running transaction check
    ---> Package containerd.io.x86_64 0:1.2.2-3.3.el7 will be installed
    ---> Package docker-ce.x86_64 3:18.09.2-3.el7 will be installed
    --> Processing Dependency: container-selinux >= 2.9 for package: 3:docker-ce-18.09.2-3.el7.x86_64
    ---> Package docker-ce-cli.x86_64 1:18.09.2-3.el7 will be installed
    --> Running transaction check
    ---> Package container-selinux.noarch 2:2.74-1.el7 will be installed
    --> Finished Dependency Resolution

    Dependencies Resolved

    ======================================================================================================================
     Package                        Arch                Version                       Repository                     Size
    ======================================================================================================================
    Installing:
     containerd.io                  x86_64              1.2.2-3.3.el7                 docker-ce-stable               22 M
     docker-ce                      x86_64              3:18.09.2-3.el7               docker-ce-stable               19 M
     docker-ce-cli                  x86_64              1:18.09.2-3.el7               docker-ce-stable               14 M
    Installing for dependencies:
     container-selinux              noarch              2:2.74-1.el7                  extras                         38 k

    Transaction Summary
    ======================================================================================================================
    Install  3 Packages (+1 Dependent package)

    Total download size: 55 M
    Installed size: 235 M
    Is this ok [y/d/N]: y
    Downloading packages:
    (1/4): container-selinux-2.74-1.el7.noarch.rpm                                                 |  38 kB  00:00:00
    warning: /var/cache/yum/x86_64/7/docker-ce-stable/packages/containerd.io-1.2.2-3.3.el7.x86_64.rpm: Header V4 RSA/SHA512 Signature, key ID 621e9f35: NOKEY
    Public key for containerd.io-1.2.2-3.3.el7.x86_64.rpm is not installed
    (2/4): containerd.io-1.2.2-3.3.el7.x86_64.rpm                                                  |  22 MB  00:00:04
    (3/4): docker-ce-18.09.2-3.el7.x86_64.rpm                                                      |  19 MB  00:00:04
    (4/4): docker-ce-cli-18.09.2-3.el7.x86_64.rpm                                                  |  14 MB  00:00:01
    ----------------------------------------------------------------------------------------------------------------------
    Total                                                                                 9.1 MB/s |  55 MB  00:00:06
    Retrieving key from https://download.docker.com/linux/centos/gpg
    Importing GPG key 0x621E9F35:
     Userid     : "Docker Release (CE rpm) <docker@docker.com>"
     Fingerprint: 060a 61c5 1b55 8a7f 742b 77aa c52f eb6b 621e 9f35
     From       : https://download.docker.com/linux/centos/gpg
    Is this ok [y/N]: y
    Running transaction check
    Running transaction test
    Transaction test succeeded
    Running transaction
      Installing : 2:container-selinux-2.74-1.el7.noarch                                                              1/4
    setsebool:  SELinux is disabled.
      Installing : containerd.io-1.2.2-3.3.el7.x86_64                                                                 2/4
      Installing : 1:docker-ce-cli-18.09.2-3.el7.x86_64                                                               3/4
      Installing : 3:docker-ce-18.09.2-3.el7.x86_64                                                                   4/4
      Verifying  : 3:docker-ce-18.09.2-3.el7.x86_64                                                                   1/4
      Verifying  : 1:docker-ce-cli-18.09.2-3.el7.x86_64                                                               2/4
      Verifying  : containerd.io-1.2.2-3.3.el7.x86_64                                                                 3/4
      Verifying  : 2:container-selinux-2.74-1.el7.noarch                                                              4/4

    Installed:
      containerd.io.x86_64 0:1.2.2-3.3.el7    docker-ce.x86_64 3:18.09.2-3.el7    docker-ce-cli.x86_64 1:18.09.2-3.el7

    Dependency Installed:
      container-selinux.noarch 2:2.74-1.el7

    Complete!
    [jp7fkf@jp7fkf_0 ~]$ yum list docker-ce --showduplicates | sort -r
     * updates: ftp.iij.ad.jp
     * remi-safe: ftp.riken.jp
    Loaded plugins: fastestmirror, langpacks
    Installed Packages
     * extras: ftp.iij.ad.jp
     * epel: www.ftp.ne.jp
    docker-ce.x86_64            3:18.09.2-3.el7                    docker-ce-stable
    docker-ce.x86_64            3:18.09.2-3.el7                    @docker-ce-stable
    docker-ce.x86_64            3:18.09.1-3.el7                    docker-ce-stable
    docker-ce.x86_64            3:18.09.0-3.el7                    docker-ce-stable
    docker-ce.x86_64            18.06.3.ce-3.el7                   docker-ce-stable
    docker-ce.x86_64            18.06.2.ce-3.el7                   docker-ce-stable
    docker-ce.x86_64            18.06.1.ce-3.el7                   docker-ce-stable
    docker-ce.x86_64            18.06.0.ce-3.el7                   docker-ce-stable
    docker-ce.x86_64            18.03.1.ce-1.el7.centos            docker-ce-stable
    docker-ce.x86_64            18.03.0.ce-1.el7.centos            docker-ce-stable
    docker-ce.x86_64            17.12.1.ce-1.el7.centos            docker-ce-stable
    docker-ce.x86_64            17.12.0.ce-1.el7.centos            docker-ce-stable
    docker-ce.x86_64            17.09.1.ce-1.el7.centos            docker-ce-stable
    docker-ce.x86_64            17.09.0.ce-1.el7.centos            docker-ce-stable
    docker-ce.x86_64            17.06.2.ce-1.el7.centos            docker-ce-stable
    docker-ce.x86_64            17.06.1.ce-1.el7.centos            docker-ce-stable
    docker-ce.x86_64            17.06.0.ce-1.el7.centos            docker-ce-stable
    docker-ce.x86_64            17.03.3.ce-1.el7                   docker-ce-stable
    docker-ce.x86_64            17.03.2.ce-1.el7.centos            docker-ce-stable
    docker-ce.x86_64            17.03.1.ce-1.el7.centos            docker-ce-stable
    docker-ce.x86_64            17.03.0.ce-1.el7.centos            docker-ce-stable
    Determining fastest mirrors
     * base: ftp.iij.ad.jp
    Available Packages
    [jp7fkf@jp7fkf_0 ~]$ sudo yum install docker-ce docker-ce-cli containerd.io
    Loaded plugins: fastestmirror, langpacks
    Loading mirror speeds from cached hostfile
     * base: ftp.nara.wide.ad.jp
     * epel: www.ftp.ne.jp
     * extras: ftp.nara.wide.ad.jp
     * remi-safe: ftp.riken.jp
     * updates: ftp.nara.wide.ad.jp
    Package 3:docker-ce-18.09.2-3.el7.x86_64 already installed and latest version
    Package 1:docker-ce-cli-18.09.2-3.el7.x86_64 already installed and latest version
    Package containerd.io-1.2.2-3.3.el7.x86_64 already installed and latest version
    Nothing to do
    [jp7fkf@jp7fkf_0 ~]$ sudo systemctl status docker
    ● docker.service - Docker Application Container Engine
       Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
       Active: inactive (dead)
         Docs: https://docs.docker.com
    [jp7fkf@jp7fkf_0 ~]$ sudo systemctl start docker
    [jp7fkf@jp7fkf_0 ~]$ sudo docker ps -a
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
  ```

## centosでsystemctl すると怒られが発生する．
  - はい．
  ```
    [jp7fkf@jp7fkf_0 ~]$ systemctl status httpd
    Failed to get D-Bus connection: Operation not permitted
  ```
  とか言われる．
  - `--privileged` つけて `-d` して `docker exec` で入るとよろしいらしい．
  ```
    [jp7fkf@jp7fkf_0 ~]$ sudo docker pull centos:latest
    latest: Pulling from library/centos
    a02a4930cb5d: Pull complete
    Digest: sha256:184e5f35598e333bfa7de10d8fb1cebb5ee4df5bc0f970bf2b1e7c7345136426
    Status: Downloaded newer image for centos:latest
    [jp7fkf@jp7fkf_0 ~]$
    [jp7fkf@jp7fkf_0 ~]$ docker images
    Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.39/images/json: dial unix /var/run/docker.sock: connect: permission denied
    [jp7fkf@jp7fkf_0 ~]$ sudo docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    centos              latest              1e1148e4cc2c        2 months ago        202MB
    [jp7fkf@jp7fkf_0 ~]$ sudo docker run -it -d --privileged --name centos centos:latest /sbin/init
    76efffadff20224247573edb5d5a03834b3be5b9868683542a4fab766c8d55ee
    [jp7fkf@jp7fkf_0 ~]$
    [jp7fkf@jp7fkf_0 ~]$ sudo docker exec -it 76ef /bin/bash
  ```
  - https://www.tcmobile.jp/dev_blog/devtool/docker%E3%81%A7centos7%E8%B5%B7%E5%8B%95%E6%99%82%E3%81%AEsystemctl%E3%81%8C%E5%8B%95%E3%81%8B%E3%81%AA%E3%81%84%E3%81%A8%E3%81%8D/

## tips
  - `--net='none'` 状態では`docker attach`を使っての接続はできない？
    - `docker exec`で`/bin/bash`を読むなどして入ることはできる．

## Docker Build
- Dockerfileをつくってbuildする
  ```
    FROM centos #もとになるimage(docker imagesで存在する/pullしてある必要がある．)
    MAINTAINER Yudai Hashimoto <yudai.hashimoto@example.com>
    CMD echo "now running..." #docker runしたときに
    RUN yum install -y iproute curl wget bind-utils
  ```
  ```
    [jp7fkf@localhost ~]$ sudo docker build -t hogehoge . ##current dir. にあるDockerfileを読んでhogehoge というimageをつくる．
    Sending build context to Docker daemon  11.78kB
    Step 1/4 : FROM centos
     ---> 1e1148e4cxxx
    Step 2/4 : MAINTAINER Yudai Hashimoto <yudai.hashimoto@example.com>
     ---> Running in 5584276013f3
    Removing intermediate container 558427601xxx
     ---> 412c0dea5xxx
    Step 3/4 : CMD echo "now running..."
     ---> Running in e7987dfe0xxx
    Removing intermediate container e7987dfe0xxx
     ---> cf4eebd0bxxx
    Step 4/4 : RUN yum install -y iproute curl wget bind-utils
     ---> Running in 65094f874a93
    Loaded plugins: fastestmirror, ovl
    Determining fastest mirrors
     * base: ftp.jaist.ac.jp
     * extras: ftp.jaist.ac.jp
     * updates: ftp.jaist.ac.jp
    Package curl-7.29.0-51.el7.x86_64 already installed and latest version
    Resolving Dependencies
    --> Running transaction check
    ---> Package bind-utils.x86_64 32:9.9.4-73.el7_6 will be installed
    --> Processing Dependency: bind-libs = 32:9.9.4-73.el7_6 for package: 32:bind-utils-9.9.4-73.el7_6.x86_64
    --> Processing Dependency: liblwres.so.90()(64bit) for package: 32:bind-utils-9.9.4-73.el7_6.x86_64
    --> Processing Dependency: libisccfg.so.90()(64bit) for package: 32:bind-utils-9.9.4-73.el7_6.x86_64
    --> Processing Dependency: libisccc.so.90()(64bit) for package: 32:bind-utils-9.9.4-73.el7_6.x86_64
    --> Processing Dependency: libisc.so.95()(64bit) for package: 32:bind-utils-9.9.4-73.el7_6.x86_64
    --> Processing Dependency: libdns.so.100()(64bit) for package: 32:bind-utils-9.9.4-73.el7_6.x86_64
    --> Processing Dependency: libbind9.so.90()(64bit) for package: 32:bind-utils-9.9.4-73.el7_6.x86_64
    --> Processing Dependency: libGeoIP.so.1()(64bit) for package: 32:bind-utils-9.9.4-73.el7_6.x86_64
    ---> Package iproute.x86_64 0:4.11.0-14.el7 will be installed
    --> Processing Dependency: libmnl.so.0(LIBMNL_1.0)(64bit) for package: iproute-4.11.0-14.el7.x86_64
    --> Processing Dependency: libxtables.so.10()(64bit) for package: iproute-4.11.0-14.el7.x86_64
    --> Processing Dependency: libmnl.so.0()(64bit) for package: iproute-4.11.0-14.el7.x86_64
    ---> Package wget.x86_64 0:1.14-18.el7 will be installed
    --> Running transaction check
    ---> Package GeoIP.x86_64 0:1.5.0-13.el7 will be installed
    ---> Package bind-libs.x86_64 32:9.9.4-73.el7_6 will be installed
    --> Processing Dependency: bind-license = 32:9.9.4-73.el7_6 for package: 32:bind-libs-9.9.4-73.el7_6.x86_64
    ---> Package iptables.x86_64 0:1.4.21-28.el7 will be installed
    --> Processing Dependency: libnfnetlink.so.0()(64bit) for package: iptables-1.4.21-28.el7.x86_64
    --> Processing Dependency: libnetfilter_conntrack.so.3()(64bit) for package: iptables-1.4.21-28.el7.x86_64
    ---> Package libmnl.x86_64 0:1.0.3-7.el7 will be installed
    --> Running transaction check
    ---> Package bind-license.noarch 32:9.9.4-72.el7 will be updated
    ---> Package bind-license.noarch 32:9.9.4-73.el7_6 will be an update
    ---> Package libnetfilter_conntrack.x86_64 0:1.0.6-1.el7_3 will be installed
    ---> Package libnfnetlink.x86_64 0:1.0.1-4.el7 will be installed
    --> Finished Dependency Resolution

    Dependencies Resolved

    ================================================================================
     Package                    Arch       Version                Repository   Size
    ================================================================================
    Installing:
     bind-utils                 x86_64     32:9.9.4-73.el7_6      updates     206 k
     iproute                    x86_64     4.11.0-14.el7          base        763 k
     wget                       x86_64     1.14-18.el7            base        547 k
    Installing for dependencies:
     GeoIP                      x86_64     1.5.0-13.el7           base        1.5 M
     bind-libs                  x86_64     32:9.9.4-73.el7_6      updates     1.0 M
     iptables                   x86_64     1.4.21-28.el7          base        433 k
     libmnl                     x86_64     1.0.3-7.el7            base         23 k
     libnetfilter_conntrack     x86_64     1.0.6-1.el7_3          base         55 k
     libnfnetlink               x86_64     1.0.1-4.el7            base         26 k
    Updating for dependencies:
     bind-license               noarch     32:9.9.4-73.el7_6      updates      87 k

    Transaction Summary
    ================================================================================
    Install  3 Packages (+6 Dependent packages)
    Upgrade             ( 1 Dependent package)

    Total download size: 4.6 M
    Downloading packages:
    Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
    warning: /var/cache/yum/x86_64/7/updates/packages/bind-license-9.9.4-73.el7_6.noarch.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
    Public key for bind-license-9.9.4-73.el7_6.noarch.rpm is not installed
    Public key for libmnl-1.0.3-7.el7.x86_64.rpm is not installed
    --------------------------------------------------------------------------------
    Total                                              1.1 MB/s | 4.6 MB  00:04
    Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    Importing GPG key 0xF4A80EB5:
     Userid     : "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>"
     Fingerprint: 6341 ab27 53d7 8a78 a7c2 7bb1 24c6 a8a7 f4a8 0eb5
     Package    : centos-release-7-6.1810.2.el7.centos.x86_64 (@CentOS)
     From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    Running transaction check
    Running transaction test
    Transaction test succeeded
    Running transaction
      Installing : GeoIP-1.5.0-13.el7.x86_64                                   1/11
      Installing : libmnl-1.0.3-7.el7.x86_64                                   2/11
      Installing : libnfnetlink-1.0.1-4.el7.x86_64                             3/11
      Installing : libnetfilter_conntrack-1.0.6-1.el7_3.x86_64                 4/11
      Installing : iptables-1.4.21-28.el7.x86_64                               5/11
      Updating   : 32:bind-license-9.9.4-73.el7_6.noarch                       6/11
      Installing : 32:bind-libs-9.9.4-73.el7_6.x86_64                          7/11
      Installing : 32:bind-utils-9.9.4-73.el7_6.x86_64                         8/11
      Installing : iproute-4.11.0-14.el7.x86_64                                9/11
      Installing : wget-1.14-18.el7.x86_64                                    10/11
    install-info: No such file or directory for /usr/share/info/wget.info.gz
      Cleanup    : 32:bind-license-9.9.4-72.el7.noarch                        11/11
      Verifying  : libnfnetlink-1.0.1-4.el7.x86_64                             1/11
      Verifying  : 32:bind-license-9.9.4-73.el7_6.noarch                       2/11
      Verifying  : libmnl-1.0.3-7.el7.x86_64                                   3/11
      Verifying  : GeoIP-1.5.0-13.el7.x86_64                                   4/11
      Verifying  : 32:bind-libs-9.9.4-73.el7_6.x86_64                          5/11
      Verifying  : wget-1.14-18.el7.x86_64                                     6/11
      Verifying  : 32:bind-utils-9.9.4-73.el7_6.x86_64                         7/11
      Verifying  : libnetfilter_conntrack-1.0.6-1.el7_3.x86_64                 8/11
      Verifying  : iproute-4.11.0-14.el7.x86_64                                9/11
      Verifying  : iptables-1.4.21-28.el7.x86_64                              10/11
      Verifying  : 32:bind-license-9.9.4-72.el7.noarch                        11/11

    Installed:
      bind-utils.x86_64 32:9.9.4-73.el7_6       iproute.x86_64 0:4.11.0-14.el7
      wget.x86_64 0:1.14-18.el7

    Dependency Installed:
      GeoIP.x86_64 0:1.5.0-13.el7
      bind-libs.x86_64 32:9.9.4-73.el7_6
      iptables.x86_64 0:1.4.21-28.el7
      libmnl.x86_64 0:1.0.3-7.el7
      libnetfilter_conntrack.x86_64 0:1.0.6-1.el7_3
      libnfnetlink.x86_64 0:1.0.1-4.el7

    Dependency Updated:
      bind-license.noarch 32:9.9.4-73.el7_6

    Complete!
    Removing intermediate container 65094f874xxx
     ---> 9f7416db3xxx
    Successfully built xxxxx
    Successfully tagged hogehoge:latest
    [jp7fkf@localhost ~]$
  ```

## docker にnetnsでnic生やす
  - 生やしたいこともある．netns, iplink勉強しなきゃね．
  ```
    sudo docker network create --driver=bridge --subnet=172.16.156.0/24 --gateway=172.16.156.1 -o "com.docker.network.bridge.name=br_eth1" br_eth1
    sudo docker run -it -d --name hoge --network="br_eth1" --ip=172.16.156.40 test_probe /bin/bash
    sudo docker start hoge
    sudo brctl addif br_eth1 eth1
  ```

  ```
      #!/bin/bash -eux
      CONTAINER_ID=$1
      IP_CONTAINER=$2
      IP_HOST=$3
      NIC_NAME=$4 

      sudo yum -y install bridge-utils
      sudo yum -y install git
      sudo git clone https://github.com/jpetazzo/pipework.git /usr/pipewor
      sudo ln -s ~/pipework/pipework /usr/bin/pipework
      pipework br0 $CONTAINER_ID $IP_CONTAINER
      ip addr add $IP_HOST dev br0
      brctl addif br0 $NIC_NAME
  ```

  - 結局のところ `sudo pipework --direct-phys eth1 $CONTAINERID 192.168.1.2/24` とかを使うのが一番早い件．さすが．
    ```
      [jp7fkf@localhost ~]$ sudo sh -x pipework/pipework --direct-phys eth1 39d4 172.16.156.40/24
      + set -e
      + case "$1" in
      + DIRECT_PHYS=1
      + shift
      + IFNAME=eth1
      + CONTAINER_IFNAME=
      + '[' 39d4 = -i ']'
      + '[' 39d4 = -l ']'
      + FAMILY_FLAG=-4
      + '[' 39d4 = -a ']'
      + GUESTNAME=39d4
      + IPADDR=172.16.156.40/24
      + MACADDR=
      + case "$MACADDR" in
      + VLAN=
      + case "$MACADDR" in
      + '[' 172.16.156.40/24 ']'
      + '[' -z '' ']'
      + '[' -d /sys/class/net/eth1 ']'
      + '[' -d /sys/class/net/eth1/bridge ']'
      + installed ovs-vsctl
      + command -v ovs-vsctl
      ++ cat /sys/class/net/eth1/type
      + '[' 1 -eq 32 ']'
      + IFTYPE=phys
      + CONTAINER_IFNAME=eth1
      + '[' '' ']'
      + '[' phys = bridge ']'
      + '[' phys = ipoib ']'
      + read _ mnt fstype options _
      + '[' rootfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' sysfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' proc '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' devtmpfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' securityfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' tmpfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' devpts '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' tmpfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' tmpfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' cgroup '!=' cgroup ']'
      + grep -qw devices
      + echo rw,nosuid,nodev,noexec,relatime,xattr,release_agent=/usr/lib/systemd/systemd-cgroups-agent,name=systemd
      + continue
      + read _ mnt fstype options _
      + '[' pstore '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' cgroup '!=' cgroup ']'
      + echo rw,nosuid,nodev,noexec,relatime,cpuacct,cpu
      + grep -qw devices
      + continue
      + read _ mnt fstype options _
      + '[' cgroup '!=' cgroup ']'
      + grep -qw devices
      + echo rw,nosuid,nodev,noexec,relatime,pids
      + continue
      + read _ mnt fstype options _
      + '[' cgroup '!=' cgroup ']'
      + echo rw,nosuid,nodev,noexec,relatime,freezer
      + grep -qw devices
      + continue
      + read _ mnt fstype options _
      + '[' cgroup '!=' cgroup ']'
      + grep -qw devices
      + echo rw,nosuid,nodev,noexec,relatime,cpuset
      + continue
      + read _ mnt fstype options _
      + '[' cgroup '!=' cgroup ']'
      + echo rw,nosuid,nodev,noexec,relatime,memory
      + grep -qw devices
      + continue
      + read _ mnt fstype options _
      + '[' cgroup '!=' cgroup ']'
      + grep -qw devices
      + echo rw,nosuid,nodev,noexec,relatime,hugetlb
      + continue
      + read _ mnt fstype options _
      + '[' cgroup '!=' cgroup ']'
      + echo rw,nosuid,nodev,noexec,relatime,perf_event
      + grep -qw devices
      + continue
      + read _ mnt fstype options _
      + '[' cgroup '!=' cgroup ']'
      + grep -qw devices
      + echo rw,nosuid,nodev,noexec,relatime,net_prio,net_cls
      + continue
      + read _ mnt fstype options _
      + '[' cgroup '!=' cgroup ']'
      + echo rw,nosuid,nodev,noexec,relatime,blkio
      + grep -qw devices
      + continue
      + read _ mnt fstype options _
      + '[' cgroup '!=' cgroup ']'
      + grep -qw devices
      + echo rw,nosuid,nodev,noexec,relatime,devices
      + CGROUPMNT=/sys/fs/cgroup/devices
      + read _ mnt fstype options _
      + '[' configfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' xfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' mqueue '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' debugfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' hugetlbfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' xfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' autofs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' overlay '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' tmpfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' proc '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' tmpfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' overlay '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' tmpfs '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' proc '!=' cgroup ']'
      + continue
      + read _ mnt fstype options _
      + '[' /sys/fs/cgroup/devices ']'
      ++ find /sys/fs/cgroup/devices -name 39d4
      ++ wc -l
      + N=0
      + case "$N" in
      + installed docker
      + command -v docker
      + RETRIES=3
      + '[' 3 -gt 0 ']'
      ++ docker inspect '--format={{ .State.Pid }}' 39d4
      + DOCKERPID=29121
      ++ docker inspect '--format={{ .ID }}' 39d4
      + DOCKERCID=39d4a4146861624830f60bd15f07c61215ee7cd10af3063e543b48baeedbxxxx
      ++ docker inspect '--format={{ .Name }}' 39d4
      + DOCKERCNAME=/hoge
      + '[' 29121 '!=' 0 ']'
      + break
      + '[' 29121 = 0 ']'
      + '[' 29121 = '<no value>' ']'
      + '[' phys '!=' route ']'
      + '[' phys '!=' rule ']'
      + '[' phys '!=' tc ']'
      + case "$IPADDR" in
      + :
      + '[' '' ']'
      + '[' 29121 ']'
      + NSPID=29121
      + '[' phys = phys ']'
      + '[' '' ']'
      + '[' '!' -d /var/run/netns ']'
      + rm -f /var/run/netns/29121
      + ln -s /proc/29121/ns/net /var/run/netns/29121
      + '[' phys = bridge ']'
      + '[' phys '!=' route ']'
      + '[' phys '!=' dummy ']'
      + '[' phys '!=' rule ']'
      + '[' phys '!=' tc ']'
      ++ ip link show eth1
      ++ awk '{print $5}'
      + MTU=1500
      + '[' phys = bridge ']'
      + '[' phys = phys ']'
      + '[' '' ']'
      + '[' '!' -z 1 ']'
      + GUEST_IFNAME=eth1
      + ip link set eth1 up
      + '[' phys = ipoib ']'
      + '[' phys = dummy ']'
      + '[' phys = route ']'
      + '[' phys = rule ']'
      + '[' phys = tc ']'
      + ip link set eth1 netns 29121
      + ip netns exec 29121 ip link set eth1 name eth1
      + '[' '' ']'
      + case "$DHCP_CLIENT" in
      + installed ipcalc
      + command -v ipcalc
      ++ ipcalc -b 172.16.156.40/24
      + eval BROADCAST=172.16.156.255
      ++ BROADCAST=172.16.156.255
      + ip netns exec 29121 ip -4 addr add 172.16.156.40/24 brd 172.16.156.255 dev eth1
      + '[' '' ']'
      + ip netns exec 29121 ip -4 link set eth1 up
      + '[' '' ']'
      + installed arping
      + command -v arping
      ++ echo 172.16.156.40/24
      ++ cut -d/ -f1
      + IPADDR=172.16.156.40
      + ip netns exec 29121 arping -c 1 -A -I eth1 172.16.156.40
      + rm -f /var/run/netns/29121
      [jp7fkf@localhost ~]$
    ````