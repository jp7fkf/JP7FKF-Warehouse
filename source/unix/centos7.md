# CentOS7

## nmcli
- デバイスを確認  
  - `nmcli d`

- 必要に応じて
  - `nmcli connection add type <デバイスタイプ> ifname <ifname名> con-name <接続名>`
  - ex.) `nmcli connection add type ethernet ifname ens192 con-name ens192`

- インターフェイスを有効化
  - `nmcli c m eth0 connection.autoconnect yes`

- IPアドレス、サブネットマスクを変更  
  - `nmcli c modify eth0 ipv4.addresses 172.16.0.100/24 `  
※デバイスを確認して「eth0」をnmcli 

- デフォルトゲー connectiondnsnmcli c downトウェイを設定  
  - `nmcli c modify eth0 ipv4.gateway 172.16.0.1`

- DNS設定  
  - `nmcli c modify eth0 ipv4.dns 172.16.0.1`

- IPを固定割り当てに設定 (DHCP は "auto"に変更)  
  - `nmcli c modify eth0 ipv4.method manual`

- インターフェースを再起動して設定を反映  
  - `nmcli c down eth0; nmcli c up eth0`  
※デバイスを確認して「eth0」を変更する

- 設定確認  
  - `nmcli d show eth0`

- 接続確認  
  - `ip addr`

- ネットワーク再起動  
  - `systemctl restart network`

- ファイル確認  
  - `less /etc/sysconfig/network-scripts/ifcfg-eth0`  
※ifcfg-eth0はデバイスで確認する。  

## network-script/ifcfgを直接編集する方式．

## yum
  - `yum list`: インストール可能 || インストール済みパッケージ を表示
  - `yum list installed` : インストール済みパッケージを表示
  - `yum --showduplicates search <package_name>`: 重複も含めてinstall可能なpackageを取得．
    - ここで得られたバージョン等を含めて`yum install`することでバージョンを指定してinstallが可能．

## rpm
  - `$ rpm -i`: インストール
  - `$ rpm -U`: アップデート

## kernel update
###基本的な手順
1. 現在のカーネル・パッケージの確認
2. 最新のカーネル・パッケージの入手
3. 入手したカーネル・パッケージの適用
4. GRUBの更新
5. マシンの再起動

### CentOS6
- kernel version 確認: `$ less /proc/version`
- kernel version 確認: `$ uname -r`
- 入ってるkernelを確認: `$ rpm -qa | grep kernel`
- wget とかでパッケージを持ってくる． `$ wget https://elrepo.org/linux/kernel/el6/x86_64/RPMS/hogehoge /tmp/`
- grubでどのkernelからあげるかをset: `/etc/grub.conf`のうちのn番目のkernelで起動するときは，`$ # /sbin/grubby --set-default=<n-1>`
- (参考)カーネルレポジトリ: https://elrepo.org/linux/kernel/el6/x86_64/RPMS/

- kernel-<バージョン番号>: ユニプロセッサマシン向けカーネル(≒カーネル本体か．)
- kernel-smp-<バージョン番号>: マルチプロセッサ、または、4GB以上のメモリを搭載しているマシン向けカーネル
- kernel-hugemem-<バージョン番号>: 16GB以上の大規模なメモリを搭載しているマシン向けカーネル
- kernel-doc-<バージョン番号>: カーネルソース内のドキュメントやカーネルに関連したドキュメント
- kernel-source-<バージョン番号>: カーネルソース一式。サードパーティあるいはオリジナルのドライバをビルドする場合に必要になります。
- kernel-devel: サードパーティあるいはオリジナルのドライバをビルドする場合に最低限必要になるファイル一式。ヘッダファイル、makefile等で構成される。

### 参考
- [2. カーネルのアップデート(第1章カーネル:基本管理コースII)](https://users.miraclelinux.com/technet/document/linux/training/2_1_2.html)


## Chkconfig
- ランレベル
```
0: システムの停止
1: シングルユーザモード
2: マルチユーザモード
3: マルチユーザモード(コンソールログイン)
4: 未使用
5: マルチユーザモード(ディスプレイマネージャ使用)
6: システム再起動
```

- commands
```
chkconfig --add <service_name>
chkconfig --del <service_name>
chkconfig --list
chkconfig --list <service_name>
chkconfig --level <run_level> <service_name> <on|off>
```


## kernel update via yum
```
  rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
    ※正常なら何も表示されない
  rpm -Uvh https://www.elrepo.org/elrepo-release-6-8.el6.elrepo.noarch.rpm
  yum --enablerepo=elrepo-kernel -y install kernel-lt
  rpm -qa | grep "^kernel" | sort
  yum --enablerepo=elrepo-kernel -y swap \kernel-headers -- \kernel-lt-headers


```

```
wget --no-check-certificate https://elrepo.org/linux/kernel/el6/x86_64/RPMS/kernel-lt-4.4.169-1.el6.elrepo.x86_64.rpm
wget --no-check-certificate https://elrepo.org/linux/kernel/el6/x86_64/RPMS/kernel-lt-devel-4.4.169-1.el6.elrepo.x86_64.rpm
wget --no-check-certificate https://elrepo.org/linux/kernel/el6/x86_64/RPMS/kernel-lt-doc-4.4.169-1.el6.elrepo.noarch.rpm
wget --no-check-certificate https://elrepo.org/linux/kernel/el6/x86_64/RPMS/kernel-lt-headers-4.4.169-1.el6.elrepo.x86_64.rpm
```
```
rpm -e --nodeps kernel-headers
yum list installed "kernel-*"
```
## motd, issueをかく
  - これを書いておくと，サーバの役割だとか，みんなに伝えたいことがかけてよい．
  - motd(message of the day)
    - `/etc/motd` に書く．
  - issue
    - consoleのときの表示は `/etc/issue` に書く
    - remote loginのときの表示は `/etc/issue.net` に書く．
  - banner
    - sshのときのbannerは `/etc/ssh/sshd_conf` のBannerに書く．

## pyenv install
```
  yum -y install gcc zlib-devel bzip2 bzip2-devel readline readline-devel sqlite sqlite-devel openssl openssl-devel libffi-devel
  # python 3.7系をinstallするときは libffi-devel が求められる．
  curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash

  vim ~/.bashrc
  # 下記を記載．
  # export PATH="~/.pyenv/bin:$PATH"
  # eval "$(pyenv init -)"
  # eval "$(pyenv virtualenv-init -)"

  source ~/.bashrc
  pyenv -v
```


## /boot 配下が100%で何もできない件
- /boot配下にゴミが残っていていっぱいになっているとapt installとかができなくなる．
```
user@centos:~$ sudo apt-get autoremove
パッケージリストを読み込んでいます... 完了
依存関係ツリーを作成しています
状態情報を読み取っています... 完了
これらを直すためには 'apt-get -f install' を実行する必要があるかもしれません。
以下のパッケージには満たせない依存関係があります:
 linux-image-extra-4.4.0-141-generic : 依存: linux-image-4.4.0-141-generic しかし、インストールされていません
 linux-image-generic : 依存: linux-image-4.4.0-141-generic しかし、インストールされていません
E: 未解決の依存関係があります。-f オプションを試してください。

user@centos:~$ sudo apt-get purge linux-image-3.16.0-30-generic
パッケージリストを読み込んでいます... 完了
依存関係ツリーを作成しています
状態情報を読み取っています... 完了
以下の問題を解決するために 'apt-get -f install' を実行する必要があるかもしれません:
以下のパッケージには満たせない依存関係があります:
 linux-image-extra-3.16.0-30-generic : 依存: linux-image-3.16.0-30-generic しかし、インストールされようとしていません
 linux-image-extra-4.4.0-141-generic : 依存: linux-image-4.4.0-141-generic しかし、インストールされようとしていません
 linux-image-generic : 依存: linux-image-4.4.0-141-generic しかし、インストールされようとしていません
E: 未解決の依存関係です。'apt-get -f install' を実行してみてください (または解法を明示してください)。

user@centos:~$ df
Filesystem                     1K-blocks     Used Available Use% Mounted on
udev                            24686352        0  24686352   0% /dev
tmpfs                            4941256     9256   4932000   1% /run
/dev/mapper/centos-hoge--vg-root 526432040 59621920 440045832  12% /
tmpfs                           24706264        0  24706264   0% /dev/shm
tmpfs                               5120        0      5120   0% /run/lock
tmpfs                           24706264        0  24706264   0% /sys/fs/cgroup
/dev/sda1                         240972   228913         0 100% /boot
cgmfs                                100        0       100   0% /run/cgmanager/fs
tmpfs                            4941256        0   4941256   0% /run/user/1001
```
- この時点で`apt-get -f install`しても`/boot`が足りてないせいで正常終了しない．
- というわけで/boot配下を掃除する．
  - https://gist.github.com/ipbastola/2760cfc28be62a5ee10036851c654600
    - ``sudo dpkg --list 'linux-image*'|awk '{ if ($1=="ii") print $2}'|grep -v `uname -r``
      - すると
      ```user@centos:~$ sudo dpkg --list 'linux-image*'|awk '{ if ($1=="ii") print $2}'|grep -v `uname -r`
      [sudo] password for user:
      linux-image-3.16.0-30-generic
      linux-image-4.4.0-130-generic
      linux-image-4.4.0-133-generic
      linux-image-extra-3.16.0-30-generic
      linux-image-extra-4.4.0-130-generic
      linux-image-extra-4.4.0-133-generic
      ```
    - `sudo rm -rf /boot/{消したいimage}`
    - `sudo apt-get -f install`
    - `sudo apt-get autoremove`
    - これで`df`するととりあえずavailableは増えていると思われるので，あとはなんとかなる．
    - `sudo update-grub`
    - `sudo apt-get update`

## yumとrpmって競合するん？
- 基本的にしないっぽい．yumは依存性解決とかをやってくれているだけで，中ではrpm叩いてるらしい．

## yum とかrpmでURL指定でinstallする
- https://qiita.com/kazinoue/items/d432b858c71618a1c15e
- `sudo rpm -[i,U]vh https://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-9.noarch.rpm`
- `sudo yum install http://ftp.riken.jp/Linux/centos/7.5.1804/updates/x86_64/Packages/gcc-4.8.5-28.el7_5.1.x86_64.rpm`

## yumでダウンロードされたファイル
- `/var/cache/yum/base/packages/`
  - 基本的にここにあるっぽい． `sudo yum clean` したら消えるんだろうか．

## `df` でみるといっぱいなのに `/` 配下にはそんなにない．
- `/tmp`とかが別パーティでマウントされてたりすると．．．
- `sudo lsof / | sort -k7 -nr | head -1` するといちばんやべーのがでてくる．

## yum install/update したあとにprocess/service/system restartが必要なのか？
- `needs-restarting` コマンドというのがある．
- `needs-restarting`: リスタートすべきプロセスが出る．
- `needs-restarting -s`: リスタートすべきサービスが出る．
- `needs-restarting -r`: システムリスタートが必要かをみる．
- これは`yum-utils`パッケージに入ってる．
