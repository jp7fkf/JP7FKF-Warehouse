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

### LEDがおかしい．
```
show chassis alarms 
show system alarms 
show chassis led 
```
- 何も考えずに`show chassis` 系と`show system` 系を叩くんだな．

## commit and-quit
  - commit成功したらそのまま抜けれて便利

## commit commnet <text>
  - コミットにコメントがつく
  - `show system commit` とかするときにわかりやすい．

## show system rollback compare <commit1> <commit2>
  - 1と2の差分を見る．

## configure exclusive
  - 誰にも編集させないぞ

## configのload
  - `load set terminal`
    - set 形式でloadできる．

  - `load merge terminal`
    - json 形式そのままload，既存のやつはmergeされるんだろうな．たぶん．

  - `load override terminal`
    - json 形式そのままload. 既存のやつをoverrideするんだろうな．たぶん．
      - だから編集した階層になにか既存configあったら消えちゃうから注意っぽい．

## interface-range
  ```
  set interfaces interface-range VLAN100-grp member-range ge-0/0/1 to ge-0/0/11
  set interfaces interface-range VLAN100-grp unit 0 family ethernet-switching vlan members 100
  ```
  - 上記のようにinterfaceをまとめて設定できる．
  - configにもrangeのまとまりで反映される．
  ```
  interfaces {
    interface-range VLAN100-int {
        member-range ge-0/0/1 to ge-0/0/11;
        unit 0 {
            family ethernet-switching {
                vlan {
                    members 100;
                }
            }
        }
    }
  }
    ```

## wildcard range {set | delete} hogehoge [0-5] hugahuga
  - ex). `$ wildcard range set interfaces ge-0/0/[0-10] unit 0 family ethernet-switching access`
  - ワイルドかードでいれれて便利．
  - `!`not でexceptもできる．
  - `interface-range`との違いは展開されるかどうか．`interface-range`では展開されずにそのままconfigに入るが，これはset形式に展開されて入るので普通にsetで1つ1つのinterfaceに対して設定したのと同じconfigが入ることになる．

## monitor log message
  - `terminal monitor` です．

## replace pattern <text1> with <text2>
  - text1 を text2 に置換．
  - 今の階層以下にしか効かない．
  - 正規表現も使える．

## rename hoge to huga
  - その階層のお名前を変える．
  - 下の階層まで消して作り直したりしなくてよくて便利


## mpls
  - http://www.mplsvpn.info/2018/04/using-salt-with-network-devices-part-2_21.html

### disable と deactivateの違い
  - http://www.networkers-online.com/blog/2016/05/junos-disable-vs-deactivate/

## traceoprionsでファイル作ったのを消したい．
  - 中身だけclear: `clear log <file_name>`
  - いやファイルごと消したいんだけど: 
    ```
      # start shell
      # rm /var/log/<file_name>
      # [y/n]
    ```

## Chassis Clustor でnodeを切り替えたいんだが．
  - `request routing-engine login node 1`
  - `root@host-A% rlogin -T node1`