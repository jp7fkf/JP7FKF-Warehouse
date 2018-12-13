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
- 

## yum
  - `$ yum list`: インストール可能 || インストール済みパッケージ を表示

  - `$ yum list installed` : インストール済みパッケージを表示

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