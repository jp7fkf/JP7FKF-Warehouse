# CentOS7

## nmcli
- デバイスを確認  
  - `nmcli d`

- インターフェイスを有効化
  - `nmcli c m eth0 connection.autoconnect yes`

- IPアドレス、サブネットマスクを変更  
  - `nmcli c modify eth0 ipv4.addresses 172.16.0.100/24 `  
※デバイスを確認して「eth0」を変更する

- デフォルトゲートウェイを設定  
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