# Ubuntu

## Ubuntu18.04

### root user
- デフォルトでは`root`はpasswordが設定されていないので利用できない．
- 初期ユーザはsudorsに入っているので，`sudo`コマンドは利用できる．
- `sudo passwd root`をすることで`root`ユーザにpasswordを設定し利用できるようになる．
- 感想: べつにsudorsにさえ入っていれば，rootユーザは使わないようにするという発想もあるよね．ただ，rootのパスワード未設定状態だとsudorsは勝手に変更できてしまうので，やっぱり適当なpasswordを設定しておくべき．

### dnsの設定をする．
- `/etc/systemd/resolved.conf` を書き換える
  - DNSの欄に記載．複数ある場合は半角スペース区切り．
  ```
  [Resolve]
  DNS=8.8.8.8 8.8.4.4
  ```
- `systemctl restart systemd-resolved` して反映
- `systemd-resolve --status` で現状の設定がみれる．
- どうやら`localhost:53` が `/etc/resolv.conf`に記載してあり(とはいえ実は`../run/systemd/resolve/stub-resolv.conf` へのシンボリックリンク)，ここに問い合わせを投げる（自らの53番に投げる).
- `localhost:53`では`systemd-resolved` serviceが起動しており，このサービスがforward等を実施し，応答を返す動きとなっている．
```
jp7fkf@lab1:~$ sudo cat /etc/systemd/resolved.conf
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.
#
# Entries in this file show the compile time defaults.
# You can change settings by editing this file.
# Defaults can be restored by simply deleting this file.
#
# See resolved.conf(5) for details

[Resolve]
DNS=8.8.8.8
#FallbackDNS=
#Domains=
#LLMNR=no
#MulticastDNS=no
#DNSSEC=no
#Cache=yes
#DNSStubListener=yes
jp7fkf@lab1:~$ systemctl restart systemd-resolved
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to restart 'systemd-resolved.service'.
Authenticating as: jp7fkf
Password:
==== AUTHENTICATION COMPLETE ===
jp7fkf@lab1:~$ systemd-resolve --status
Global
         DNS Servers: 8.8.8.8
          DNSSEC NTA: 10.in-addr.arpa
                      16.172.in-addr.arpa
                      168.192.in-addr.arpa
                      17.172.in-addr.arpa
                      18.172.in-addr.arpa
                      19.172.in-addr.arpa
                      20.172.in-addr.arpa
                      21.172.in-addr.arpa
                      22.172.in-addr.arpa
                      23.172.in-addr.arpa
                      24.172.in-addr.arpa
                      25.172.in-addr.arpa
                      26.172.in-addr.arpa
                      27.172.in-addr.arpa
                      28.172.in-addr.arpa
                      29.172.in-addr.arpa
                      30.172.in-addr.arpa
                      31.172.in-addr.arpa
                      corp
                      d.f.ip6.arpa
                      home
                      internal
                      intranet
                      lan
                      local
                      private
                      test

Link 3 (elab192)
      Current Scopes: none
       LLMNR setting: yes
MulticastDNS setting: no
      DNSSEC setting: no
    DNSSEC supported: no

Link 2 (elab160)
      Current Scopes: DNS
       LLMNR setting: yes
MulticastDNS setting: no
      DNSSEC setting: no
    DNSSEC supported: no
         DNS Servers: 8.8.8.8
jp7fkf@lab1:~$
jp7fkf@lab1:~$ netstat -tuna
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0    172 172.16.0.53:22          192.168.20.226:56440    ESTABLISHED
tcp6       0      0 :::22                   :::*                    LISTEN
udp        0      0 127.0.0.53:53           0.0.0.0:*
jp7fkf@lab1:~$ ls /etc/resolv.conf -la
lrwxrwxrwx 1 root root 39 Feb 14 09:49 /etc/resolv.conf -> ../run/systemd/resolve/stub-resolv.conf
```

### hostname を変更する
1. `sudo vim /etc/cloud/cloud.cfg`
  - `preserve_hostnameをtrueに`
  - これを実施することで再起動後も設定が保存されるようだ．
  ```
  # This will cause the set+update hostname module to not operate (if true)
  preserve_hostname: true #default: false
  ```
1. `sudo hostnamectl set-hostname yourhostname`
1. `sudo reboot`
- 必要に応じて `/etc/hosts` も編集するとよい．

## apt - advanced package tool
- `apt update`
- `apt upgrade`
- `apt full-upgrade`
- `apt search <keyword>`
- `apt install <package_name>`
- `apt remove <package_name>`
- `apt autoremove`
- `apt purge <package_name>`
  - `/etc` 配下などの設定ファイル等も削除
- `apt clean`
  - `/var/cache/apt` 配下のcacheを削除
- `apt list --installed`
  - `dpkg -l`
- `apt show <package_name>`
- `apt depends <package_name>`: 依存するパッケージを表示
- `apt rdpeneds <package_name>`: 依存されているパッケージを表示

## go をいれる
- `sudo snap install --classic go`
- `export GOPATH=$HOME/go`
- `export
PATH=$PATH:$GOPATH/bin`

## GUIをいれる
- [Ubuntu 16.04 LTS に後から GUI (X Window System) を追加する - CUBE SUGAR CONTAINER](https://blog.amedama.jp/entry/2016/11/30/155238)

## versionを調べる
- `lsb_release -a`
- `cat /etc/issue`
- `cat /etc/os-release`

## zshにする
- `apt install zsh`
- `chsh -s $(which zsh)`
