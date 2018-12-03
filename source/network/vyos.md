# vyos

- install
  - `install image`

- 公開鍵認証の設定
  - `loadkey <username> <keyfile>`
- パスワード認証を無効にする
  - `set service ssh disable-password-authentication`
- SSHv2のみを使う
  - `set service ssh protocol-version v2`

- `commit` と `save` はべつなので注意する．
- `commit-confirm [min]`も必要に応じて使いましょう．

# vyos のconfig例
```
jp7fkf@vyos:~$ show configuration commands
set firewall all-ping 'enable'
set firewall group address-group hoge address 'x.x.x.x'
set firewall name OUTSIDEtoIN default-action 'drop'
set firewall name OUTSIDEtoIN rule 10 action 'accept'
set firewall name OUTSIDEtoIN rule 10 state established 'enable'
set firewall name OUTSIDEtoIN rule 10 state related 'enable'
set firewall name OUTSIDEtoIN rule 20 action 'accept'
set firewall name OUTSIDEtoIN rule 20 destination port '80'
set firewall name OUTSIDEtoIN rule 20 protocol 'tcp'
set firewall name OUTSIDEtoIN rule 20 source group address-group 'hoge'
set firewall name OUTSIDEtoLOCAL default-action 'drop'
set firewall name OUTSIDEtoLOCAL rule 10 action 'accept'
set firewall name OUTSIDEtoLOCAL rule 10 state established 'enable'
set firewall name OUTSIDEtoLOCAL rule 10 state related 'enable'
set firewall name OUTSIDEtoLOCAL rule 20 action 'accept'
set firewall name OUTSIDEtoLOCAL rule 20 destination port 'hogehoge'
set firewall name OUTSIDEtoLOCAL rule 20 protocol 'tcp'
set firewall name OUTSIDEtoLOCAL rule 20 source address 'hogehoge'
set interfaces ethernet eth2 address '192.168.x.x/24'
set interfaces ethernet eth3 address '192.168.y.y/24'
set interfaces ethernet eth3 firewall in name 'OUTSIDEtoIN'
set interfaces ethernet eth3 firewall local name 'OUTSIDEtoLOCAL'
set interfaces loopback 'lo'
set nat source rule 1000 description 'to_internet'
set nat source rule 1000 outbound-interface 'ethx'
set nat source rule 1000 translation address 'z.z.z.z'
set protocols ospf area 0 network '192.168.x.y/24'
set protocols ospf redistribute connected metric-type '2'
set protocols ospf redistribute static metric-type '2'
set protocols static route 0.0.0.0/0 next-hop '192.168.x.y'
set service ssh 'disable-password-authentication'
set service ssh port 'xxxxx'
set system config-management commit-revisions '20'
set system console device ttyS0 speed '9600'
set system host-name 'vyos'
set system login user vyos authentication plaintext-password hogehoge
set system ntp server '0.pool.ntp.org'
set system ntp server '1.pool.ntp.org'
set system ntp server '2.pool.ntp.org'
set system package auto-sync '1'
set system package repository trusty/main components 'main'
set system package repository trusty/main distribution 'trusty'
set system package repository trusty/main password ''
set system package repository trusty/main url 'http://us.archive.ubuntu.com/ubuntu/'
set system package repository trusty/main username ''
set system package repository trusty/universe components 'universe'
set system package repository trusty/universe distribution 'trusty'
set system package repository trusty/universe password ''
set system package repository trusty/universe url 'http://us.archive.ubuntu.com/ubuntu/'
set system package repository trusty/universe username ''
set system syslog global facility all level 'notice'
set system syslog global facility protocols level 'debug'
set system time-zone 'UTC'
```