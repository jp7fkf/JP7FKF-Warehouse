# JUNOS

## ConfigのTips
  - configure exclusive
  他の人にconfigure modeに入らせない．排他的．configure exclusiveで未コミット変更があるにも関わらず抜けると，その変更は破棄される．
  複数人で変更をするときはこれをつかうとよい．

## Virtual Chassis

### non-preprovisioned mode
マスターRE(RE0)選定 起動するときにはすべてのスイッチで以下項目比較の元、マスターの選定が行われる Master 選定の優先順位:
1.マスターシップの優先順位が最も高い (0-255までの優先順位、デフォルト値は 128)
> set virtual-chassis member <member-id> mastership-priority <priority 1-255>
2.以前動作していたときにマスターに選定されていた 3.起動している時間が長い (起動している時間が1分以上違う場合) 4.MAC アドレスの小さいほう
※マスターが選定された後、マスターREと同じ選定方式により、バックアップREスイッチの選定が実施される\\


### preprovisioned mode 
```
set virtual-chassis preprovisioned
set virtual-chassis member 0 role routing-engine
set virtual-chassis member 0 serial-number 111111111111 set virtual-chassis member 1 role line-card
set virtual-chassis member 1 serial-number 222222222222 set virtual-chassis member 2 role line-card
set virtual-chassis member 2 serial-number 333333333333 set virtual-chassis member 3 role routing-engine
```

### commands
```
set interface vme unit 0 family inet address <address/mask>
set virtual-chassis member 0 mastership-priority 255
request virtual-chassis vc-port (set|delete) pic-slot <pic_no> port <port_no>

show virtual-chassis status
show virtual-chassis vc-port
request virtual-chassis renumber member-id 5 new-member-id 4

request virtual-chassis reactivate
request virtual-chassis recycle member-id <id>
request virtual-chassis renumber member-id <id> new-member-id <id>


- vcステータス確認
show virtual-chassis status
show virtual-chassis vc-port
show virtual-chassis login
```
### mixed mode vc
```
request virtual-chassis mode mixed
request system reboot
```
### おまじない
```
set chassis redundancy graceful-switchover
set routing-options nonstop-routing
set protocols layer2-control nonstop-bridging
set system commit synchronize
```