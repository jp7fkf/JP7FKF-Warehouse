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

## upgrade from cli
```
# sudo apt update
# sudo apt upgrade
# sudo apt dist-upgrade
# do-release-upgrade
```

- battle log
```
jp7fkf@lab~$ sudo apt dist-upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following security updates require Ubuntu Pro with 'esm-infra' enabled:
  screen libkrb5-3 libgssapi-krb5-2 libpython3.6-minimal libnghttp2-14
  libisccfg160 linux-libc-dev vim-common libcurl4 libldap-2.4-2 openssl
  libc6-dev libpython3.6-dev libsystemd0 strongswan-charon python2.7-minimal
  libpam-cap libpython3.6-stdlib libelf1 python3-urllib3 binutils libirs160
  bind9-host libstrongswan open-iscsi dnsutils libncurses5 python2.7 libc6
  libpython3.6 python3.6 open-vm-tools openssh-sftp-server gawk libk5crypto3
  libisc169 udev locales procps strongswan libncursesw5
  libcharon-standard-plugins libprocps6 libx11-6 python3-requests libudev1
  linux-headers-gcp libapparmor1 krb5-locales libstrongswan-standard-plugins
  python3.6-minimal binutils-x86-64-linux-gnu linux-gcp libisc-export169
  busybox-static libc-bin libtinfo5 libkrb5support0 libcap2 tar systemd-sysv
  libcap2-bin libldap-common liblwres160 vim-runtime vim libpam-systemd
  libtinfo-dev distro-info-data ncurses-term systemd libssl-dev xxd
  ncurses-bin openssh-server libx11-data openssh-client libdns-export1100
  libnss-systemd binutils-common linux-image-gcp libglib2.0-data ncurses-base
  libc-dev-bin libbinutils libpython2.7-minimal multiarch-support libgnutls30
  vim-tiny apparmor strongswan-libcharon libisccc160 libssl1.1 libbind9-160
  python3-jinja2 accountsservice python3.6-dev libdns1100 libpython2.7-stdlib
  apache2-utils curl strongswan-starter libglib2.0-0 libcurl3-gnutls
  libaccountsservice0 tzdata busybox-initramfs
Learn more about Ubuntu Pro for 18.04 at https://ubuntu.com/18-04
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
jp7fkf@lab~$
jp7fkf@lab~$ do-release-upgrade
Checking for a new Ubuntu release
Get:1 Upgrade tool signature [1,554 B]
Get:2 Upgrade tool [1,338 kB]
Fetched 1,340 kB in 0s (0 B/s)
authenticate 'focal.tar.gz' against 'focal.tar.gz.gpg'
extracting 'focal.tar.gz'
=====
Reading cache

Checking package manager

Continue running under SSH?

This session appears to be running under ssh. It is not recommended
to perform a upgrade over ssh currently because in case of failure it
is harder to recover.

If you continue, an additional ssh daemon will be started at port
'1022'.
Do you want to continue?

Continue [yN]
=====
Starting additional sshd

To make recovery in case of failure easier, an additional sshd will
be started on port '1022'. If anything goes wrong with the running
ssh you can still connect to the additional one.
If you run a firewall, you may need to temporarily open this port. As
this is potentially dangerous it's not done automatically. You can
open the port with e.g.:
'iptables -I INPUT -p tcp --dport 1022 -j ACCEPT'

To continue please press [ENTER]
=====
Reading package lists... Done
Building dependency tree
Reading state information... Done
Hit http://us-west1.gce.archive.ubuntu.com/ubuntu bionic InRelease
Hit http://us-west1.gce.archive.ubuntu.com/ubuntu bionic-updates InRelease
Hit http://us-west1.gce.archive.ubuntu.com/ubuntu bionic-backports InRelease
Hit http://security.ubuntu.com/ubuntu bionic-security InRelease
Hit http://archive.canonical.com/ubuntu bionic InRelease
Fetched 0 B in 0s (0 B/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done

Checking for installed snaps

Calculating snap size requirements
No candidate ver:  linux-image-5.4.0-1030-gcp
No candidate ver:  linux-modules-5.4.0-1030-gcp
No candidate ver:  linux-modules-extra-5.4.0-1030-gcp

Updating repository information
Get:1 http://us-west1.gce.archive.ubuntu.com/ubuntu focal InRelease [265 kB]
Get:2 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]
Get:3 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-backports InRelease [108 kB]
Get:4 http://archive.canonical.com/ubuntu focal InRelease [12.1 kB]
Get:5 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]
Get:6 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/main amd64 Packages [970 kB]
Get:7 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/main Translation-en [506 kB]
Get:8 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/main amd64 c-n-f Metadata [29.5 kB]
Get:9 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/restricted amd64 Packages [22.0 kB]
Get:10 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/restricted Translation-en [6,212 B]
Get:11 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/restricted amd64 c-n-f Metadata [392 B]
Get:12 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/universe amd64 Packages [8,628 kB]
Get:13 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/universe Translation-en [5,124 kB]
Get:14 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/universe amd64 c-n-f Metadata [265 kB]
Get:15 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/multiverse amd64 Packages [144 kB]
Get:16 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/multiverse Translation-en [104 kB]
Get:17 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/multiverse amd64 c-n-f Metadata [9,136 B]
Get:18 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages [3,092 kB]
Get:19 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates/main Translation-en [496 kB]
Get:20 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates/main amd64 c-n-f Metadata [17.2 kB]
Get:21 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates/restricted amd64 Packages [2,669 kB]
Get:22 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates/restricted Translation-en [372 kB]
Get:23 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates/restricted amd64 c-n-f Metadata [552 B]
Get:24 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates/universe amd64 Packages [1,164 kB]
Get:25 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates/universe Translation-en [279 kB]
Get:26 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates/universe amd64 c-n-f Metadata [25.7 kB]
Get:27 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates/multiverse amd64 Packages [26.1 kB]
Get:28 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates/multiverse Translation-en [7,768 B]
Get:29 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-updates/multiverse amd64 c-n-f Metadata [620 B]
Get:30 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-backports/main amd64 Packages [45.7 kB]
Get:31 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-backports/main Translation-en [16.3 kB]
Get:32 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-backports/main amd64 c-n-f Metadata [1,420 B]
Get:33 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-backports/restricted amd64 c-n-f Metadata [116 B]
Get:34 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-backports/universe amd64 Packages [25.0 kB]
Get:35 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-backports/universe Translation-en [16.3 kB]
Get:36 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-backports/universe amd64 c-n-f Metadata [880 B]
Get:37 http://us-west1.gce.archive.ubuntu.com/ubuntu focal-backports/multiverse amd64 c-n-f Metadata [116 B]
Get:38 http://archive.canonical.com/ubuntu focal/partner amd64 Packages [856 B]
Get:39 http://archive.canonical.com/ubuntu focal/partner Translation-en [384 B]
Get:40 http://security.ubuntu.com/ubuntu focal-security/main amd64 Packages [2,713 kB]
Get:41 http://security.ubuntu.com/ubuntu focal-security/main Translation-en [413 kB]
Get:42 http://security.ubuntu.com/ubuntu focal-security/main amd64 c-n-f Metadata [13.2 kB]
Get:43 http://security.ubuntu.com/ubuntu focal-security/restricted amd64 Packages [2,552 kB]
Get:44 http://security.ubuntu.com/ubuntu focal-security/restricted Translation-en [356 kB]
Get:45 http://security.ubuntu.com/ubuntu focal-security/restricted amd64 c-n-f Metadata [552 B]
Get:46 http://security.ubuntu.com/ubuntu focal-security/universe amd64 Packages [938 kB]
Get:47 http://security.ubuntu.com/ubuntu focal-security/universe Translation-en [197 kB]
Get:48 http://security.ubuntu.com/ubuntu focal-security/universe amd64 c-n-f Metadata [19.2 kB]
Get:49 http://security.ubuntu.com/ubuntu focal-security/multiverse amd64 Packages [23.9 kB]
Get:50 http://security.ubuntu.com/ubuntu focal-security/multiverse Translation-en [5,796 B]
Get:51 http://security.ubuntu.com/ubuntu focal-security/multiverse amd64 c-n-f Metadata [548 B]
Fetched 31.9 MB in 6s (5,285 kB/s)

Checking package manager
Reading package lists... Done
Building dependency tree
Reading state information... Done

Calculating the changes

Calculating the changes
No candidate ver:  linux-image-5.0.0-1033-gcp
No candidate ver:  linux-image-5.0.0-1034-gcp
No candidate ver:  linux-image-5.3.0-1018-gcp
No candidate ver:  linux-image-5.3.0-1020-gcp
No candidate ver:  linux-image-5.3.0-1026-gcp
No candidate ver:  linux-image-5.3.0-1029-gcp
No candidate ver:  linux-image-5.3.0-1030-gcp
No candidate ver:  linux-image-5.3.0-1032-gcp
No candidate ver:  linux-image-5.4.0-1030-gcp
No candidate ver:  linux-modules-5.0.0-1033-gcp
No candidate ver:  linux-modules-5.0.0-1034-gcp
No candidate ver:  linux-modules-5.3.0-1018-gcp
No candidate ver:  linux-modules-5.3.0-1020-gcp
No candidate ver:  linux-modules-5.3.0-1026-gcp
No candidate ver:  linux-modules-5.3.0-1029-gcp
No candidate ver:  linux-modules-5.3.0-1030-gcp
No candidate ver:  linux-modules-5.3.0-1032-gcp
No candidate ver:  linux-modules-5.4.0-1030-gcp
No candidate ver:  linux-modules-extra-5.3.0-1018-gcp
No candidate ver:  linux-modules-extra-5.3.0-1020-gcp
No candidate ver:  linux-modules-extra-5.3.0-1026-gcp
No candidate ver:  linux-modules-extra-5.3.0-1029-gcp
No candidate ver:  linux-modules-extra-5.3.0-1030-gcp
No candidate ver:  linux-modules-extra-5.3.0-1032-gcp
No candidate ver:  linux-modules-extra-5.4.0-1030-gcp

Do you want to start the upgrade?


1 installed package is no longer supported by Canonical. You can
still get support from the community.

6 packages are going to be removed. 165 new packages are going to be
installed. 572 packages are going to be upgraded.

You have to download a total of 494 M. This download will take about
1 minute with your connection.

Installing the upgrade can take several hours. Once the download has
finished, the process cannot be canceled.

 Continue [yN]  Details [d]
=====
Get:737 http://us-west1.gce.archive.ubuntu.com/ubuntu focal/main amd64 zerofree amd64 1.1.1-1 [8,520 B]
...
Fetched 494 MB in 6s (17.1 MB/s)

Upgrading
Fetched 0 B in 0s (0 B/s)
  MarkInstall libc6:amd64 < 2.27-3ubuntu1.6 -> 2.31-0ubuntu9.14 @ii umU Ib > FU=1
  Installing libgcc-s1 as Depends of libc6
    MarkInstall libgcc-s1:amd64 < none -> 10.5.0-1ubuntu1~20.04 @un uN Ib > FU=0
    Installing gcc-10-base as Depends of libgcc-s1
      MarkInstall gcc-10-base:amd64 < none -> 10.5.0-1ubuntu1~20.04 @un uN > FU=0
  Installing libcrypt1 as Depends of libc6
    MarkInstall libcrypt1:amd64 < none -> 1:4.4.10-10ubuntu4 @un uN > FU=0
    MarkInstall locales:amd64 < 2.27-3ubuntu1.6 -> 2.31-0ubuntu9.14 @ii umU Ib > FU=0
    Installing libc-bin as Depends of locales
      MarkInstall libc-bin:amd64 < 2.27-3ubuntu1.6 -> 2.31-0ubuntu9.14 @ii umU > FU=0
  new important dependency: libidn2-0:amd64
  Installing libidn2-0 as Recommends of libc6
    MarkInstall libidn2-0:amd64 < 2.0.4-1.1ubuntu0.2 -> 2.2.0-2 @ii umU > FU=0
Starting pkgProblemResolver with broken count: 1
Starting 2 pkgProblemResolver with broken count: 1
Investigating (0) libc6-dev:amd64 < 2.27-3ubuntu1.6 -> 2.31-0ubuntu9.14 @ii umU Ib >
Broken libc6-dev:amd64 Depends on libcrypt-dev:amd64 < none | 1:4.4.10-10ubuntu4 @un uH >
  Considering libcrypt-dev:amd64 1 as a solution to libc6-dev:amd64 18
  MarkKeep libc6-dev:amd64 < 2.27-3ubuntu1.6 -> 2.31-0ubuntu9.14 @ii umU Ib > FU=0
  Re-Instated manpages-dev:amd64
  Re-Instated libcrypt-dev:amd64
  Re-Instated libc6-dev:amd64
Done

Upgrading
Fetched 0 B in 0s (0 B/s)
Preconfiguring packages ...
Preconfiguring packages ...
Preconfiguring packages ...
Preconfiguring packages ...
Selecting previously unselected package gcc-10-base:amd64.
(Reading database ... 142481 files and directories currently installed.)
Preparing to unpack .../gcc-10-base_10.5.0-1ubuntu1~20.04_amd64.deb ...
Unpacking gcc-10-base:amd64 (10.5.0-1ubuntu1~20.04) ...

Progress: [  5%]
Setting up gcc-10-base:amd64 (10.5.0-1ubuntu1~20.04) ...
Selecting previously unselected package libgcc-s1:amd64.
(Reading database ... 142486 files and directories currently installed.)
Preparing to unpack .../libgcc-s1_10.5.0-1ubuntu1~20.04_amd64.deb ...

Progress: [ 10%]
Unpacking libgcc-s1:amd64 (10.5.0-1ubuntu1~20.04) ...
Replacing files in old package libgcc1:amd64 (1:8.4.0-1ubuntu1~18.04) ...
Setting up libgcc-s1:amd64 (10.5.0-1ubuntu1~20.04) ...

Progress: [ 16%]
(Reading database ... 142488 files and directories currently installed.)
Preparing to unpack .../locales_2.31-0ubuntu9.14_all.deb ...
Unpacking locales (2.31-0ubuntu9.14) over (2.27-3ubuntu1.6) ...

Progress: [ 21%]
Preparing to unpack .../libc6_2.31-0ubuntu9.14_amd64.deb ...
Checking for services that may need to be restarted...
Checking init scripts...
Checking for services that may need to be restarted...
Checking init scripts...
=====
 ┌────────────────────────────────┤ Configuring libc6 ├─────────────────────────────────┐
 │                                                                                      │
 │ There are services installed on your system which need to be restarted when certain  │
 │ libraries, such as libpam, libc, and libssl, are upgraded. Since these restarts may  │
 │ cause interruptions of service for the system, you will normally be prompted on      │
 │ each upgrade for the list of services you wish to restart.  You can choose this      │
 │ option to avoid being prompted; instead, all necessary restarts will be done for     │
 │ you automatically so you can avoid being asked questions on each library upgrade.    │
 │                                                                                      │
 │ Restart services during package upgrades without asking?                             │
 │                                                                                      │
 │                        <Yes>                           <No>                          │
 │                                                                                      │
 └──────────────────────────────────────────────────────────────────────────────────────┘
=====
Progress: [ 96%]
Reading package lists... Done
Building dependency tree
Reading state information... Done

Calculating the changes

Calculating the changes
=====
 ┌─────────────────────────────────┤ Configuring lxd ├──────────────────────────────────┐
 │ The LXD project puts out monthly feature releases which while backward compatible    │
 │ at an API and CLI level, will contain some behavior change and potentially require   │
 │ manual intervention during an upgrade.                                               │
 │                                                                                      │
 │ In addition to those, every 2 years a LTS release is made which comes with 5 years   │
 │ of support through frequent bugfix-only releases.                                    │
 │                                                                                      │
 │ The LXD team recommends you pick "4.0" for production environments and use "latest"  │
 │ if you're interested in getting the latest LXD features.                             │
 │                                                                                      │
 │ LXD snap track                                                                       │
 │                                                                                      │
 │                                         3.0                                          │
 │                                         4.0                                          │
 │                                                                                      │
 │                                                                                      │
 │                                        <Ok>                                          │
 │                                                                                      │
 └──────────────────────────────────────────────────────────────────────────────────────┘
=====
```
