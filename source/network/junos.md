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

## request vmhost reboot
- `request system reboot` では最近のcontrol planeがvmに乗っている機器だとhwはrebootしない．上記コマンドではhw(vmhost)ごとrebootする．

## VPLSのmacをみる．
- `show l2vpn forwarding bridge-domain VPLS:VLAN100 mac-address location 0/0/CPU0 | include c496`

## 流量を見たくなったら
- `monitor interface traffic`
- `monitor interface <interface_name>`

## sessionを消す
```
junos> clear security flow session ?
Possible completions:
  <[Enter]>            Execute this command
  all                  Clear all sessions
  application          Application protocol name
  application-firewall  Show application-firewall sessions
  application-traffic-control  Show application-traffic-control sessions
  conn-tag             Session connection tag (0..4294967295)
  destination-port     Destination port (1..65535)
  destination-prefix   Destination IP prefix or address
  family               Protocol family
  idp                  IDP sessions
  interface            Name of incoming or outgoing interface
  nat                  Sessions with network address translation
  protocol             IP protocol number
  resource-manager     Sessions with resource manager
  session-identifier   Clear session with specified session identifier
  source-port          Source port (1..65535)
  source-prefix        Source IP prefix or address
  tunnel               Tunnel sessions
  |                    Pipe through a command
```

## port checker
- https://apps.juniper.net/home/port-checker/

## vc-portのdiagnostics
- `request virtual-chassis vc-port diagnostics optics`
- `show virtual-chassis vc-port diagnostics optics`
  - `show virtual-chassis vc-port diagnostics optics `
  - `match "(fpc|vcp|Module voltage  |Receiver signal avarage|output power  |Module temperature  )"` あたりを併用すると見やすそう
    - example: 
    ```
    fpc0:
    --------------------------------------------------------------------------
    Virtual chassis port: vcp-255/2/1
        Laser output power                        :  0.4520 mW / -3.45 dBm
        Module temperature                        :  30 degrees C / 86 degrees F
        Module voltage                            :  3.3650 V
        Laser output power                        :  0.4520 mW / -3.45 dBm
        Module temperature                        :  30 degrees C / 86 degrees F
    Virtual chassis port: vcp-255/2/2
        Laser output power                        :  0.5210 mW / -2.83 dBm
        Module temperature                        :  30 degrees C / 86 degrees F
        Module voltage                            :  3.3640 V
        Laser output power                        :  0.5210 mW / -2.83 dBm
        Module temperature                        :  30 degrees C / 86 degrees F
    ```

## chassis cluster組んでる時にほかのノードにlogin
- `request routing-engine login node <node_num>`

## interface prefixes([REF](https://www.juniper.net/documentation/en_US/junos/topics/concept/interfaces-interface-naming-overview.html))
- ae: Aggregated Ethernet interface. This is a virtual aggregated link and has a different naming format from most PICs; for more information, see Aggregated Ethernet Interfaces Overview.
- as: Aggregated SONET/SDH interface. This is a virtual aggregated link and has a different naming format from most PICs; for more information, see Configuring Aggregated SONET/SDH Interfaces.
- at: ATM1 or ATM2 intelligent queuing (IQ) interface or a virtual ATM interface on a circuit emulation (CE) interface.
- bcm: The bcm0 internal Ethernet process is supported on specific Routing engines for various M series and T series routers. For more information please refer the link titled Supported Routing Engines by Chassis under Related Documentation section.
- cau4: Channelized AU-4 IQ interface (configured on the Channelized STM1 IQ or IQE PIC or Channelized OC12 IQ and IQE PICs).
- ce1: Channelized E1 IQ interface (configured on the Channelized E1 IQ PIC or Channelized STM1 IQ or IQE PIC).
- ci: Container interface.
- coc1: Channelized OC1 IQ interface (configured on the Channelized OC12 IQ and IQE or Channelized OC3 IQ and IQE PICs).
- coc3: Channelized OC3 IQ interface (configured on the Channelized OC3 IQ and IQE PICs).
- coc12: Channelized OC12 IQ interface (configured on the Channelized OC12 IQ and IQE PICs).
- coc48: Channelized OC48 interface (configured on the Channelized OC48 and Channelized OC48 IQE PICs).
- cp: Collector interface (configured on the Monitoring Services II PIC).
- cstm1: Channelized STM1 IQ interface (configured on the Channelized STM1 IQ or IQE PIC).
- cstm4: Channelized STM4 IQ interface (configured on the Channelized OC12 IQ and IQE PICs).
- cstm16: Channelized STM16 IQ interface (configured on the Channelized OC48/STM16 and Channelized OC48/STM16 IQE PICs).
- ct1: Channelized T1 IQ interface (configured on the Channelized DS3 IQ and IQE PICs, Channelized OC3 IQ and IQE PICs, Channelized OC12 IQ and IQE PICs, or Channelized T1 IQ PIC).
- ct3: Channelized T3 IQ interface (configured on the Channelized DS3 IQ and IQE PICs, Channelized OC3 IQ and IQE PICs, or Channelized OC12 IQ and IQE PICs).
- demux: Interface that supports logical IP interfaces that use the IP source or destination address to demultiplex received packets. Only one demux interface (demux0) exists per chassis. All demux logical interfaces must be associated with an underlying logical interface.
- dfc: Interface that supports dynamic flow capture processing on T Series or M320 routers containing one or more Monitoring Services III PICs. Dynamic flow capture enables you to capture packet flows on the basis of dynamic filtering criteria. Specifically, you can use this feature to forward passively monitored packet flows that match a particular filter list to one or more destinations using an on-demand control protocol.
- ds: DS0 interface (configured on the Multichannel DS3 PIC, Channelized E1 PIC, Channelized OC3 IQ and IQE PICs, Channelized OC12 IQ and IQE PICs, Channelized DS3 IQ and IQE PICs, Channelized E1 IQ PIC, Channelized STM1 IQ or IQE PIC, or Channelized T1 IQ).
- dsc: Discard interface.
- e1: E1 interface (including channelized STM1-to-E1 interfaces).
- e3: E3 interface (including E3 IQ interfaces).
- em: Management and internal Ethernet interfaces. For M Series routers, MX Series routers, T Series routers, and TX Series routers, you can use the show chassis hardware command to display hardware information about the router, including its Routing Engine model. To determine which management interface is supported on your router and Routing Engine combination, see Understanding Management Ethernet Interfaces and Supported Routing Engines by Router.
- es: Encryption interface.
- et: 100-Gigabit Ethernet interfaces (10, 40, and 100-Gigabit Ethernet interface for PTX Series Packet Transport Routers only).
- fe: Fast Ethernet interface.
- fxp: Management and internal Ethernet interfaces. For M Series routers, MX Series routers, T Series routers, and TX Series routers, you can use the show chassis hardware command to display hardware information about the router, including its Routing Engine model. To determine which management interface is supported on your router and Routing Engine combination, see Understanding Management Ethernet Interfaces and Supported Routing Engines by Router.
- ge: Gigabit Ethernet interface.
- gr: Generic routing encapsulation (GRE) tunnel interface.
- gre: Internally generated interface that is configurable only as the control channel for Generalized MPLS (GMPLS). For more information about GMPLS, see the MPLS Applications Feature Guide.
- ip: IP-over-IP encapsulation tunnel interface.
- ipip: Internally generated interface that is not configurable.
- ixgbe: The internal Ethernet process ixgbe0 and ixgbe1 are used by the RE-DUO-C2600-16G Routing Engine, which is supported on TX Matrix Plus and PTX5000.
- iw: Logical interfaces associated with the endpoints of Layer 2 circuit and Layer 2 VPN connections (pseudowire stitching Layer 2 VPNs). For more information about VPNs, see the Junos OS VPNs Library for Routing Devices.
- lc: Internally generated interface that is not configurable.
- lo: Loopback interface. The Junos OS automatically configures one loopback interface (lo0). The logical interface lo0.16383 is a nonconfigurable interface for router control traffic.
- ls: Link services interface.
- lsi: Internally generated interface that is not configurable.
- ml: Multilink interface (including Multilink Frame Relay and MLPPP).
- mo: Monitoring services interface (including monitoring services and monitoring services II). The logical interface mo-fpc/pic/port.16383 is an internally generated, nonconfigurable interface for router control traffic.
- ms: Multiservices interface.
- mt: Multicast tunnel interface (internal router interface for VPNs). If your router has a Tunnel PIC, the Junos OS automatically configures one multicast tunnel interface (mt) for each virtual private network (VPN) you configure. Although it is not necessary to configure multicast interfaces, you can use the multicast-only statement to configure the unit and family so that the tunnel can transmit and receive multicast traffic only. For more information, see multicast-only.
- mtun: Internally generated interface that is not configurable.
- oc3: OC3 IQ interface (configured on the Channelized OC12 IQ and IQE PICs or Channelized OC3 IQ and IQE PICs).
- pd: Interface on the rendezvous point (RP) that de-encapsulates packets.
- pe: Interface on the first-hop PIM router that encapsulates packets destined for the RP router.
- pimd: Internally generated interface that is not configurable.
- pime: Internally generated interface that is not configurable.
- rlsq: Container interface, numbered from 0 through 127, used to tie the primary and secondary LSQ PICs together in high availability configurations. Any failure of the primary PIC results in a switch to the secondary PIC and vice versa.
- rms: Redundant interface for two multiservices interfaces.
- rsp: Redundant virtual interface for the adaptive services interface.
- se: Serial interface (including EIA-530, V.35, and X.21 interfaces).
- si: Services-inline interface, which is hosted on a Trio-based line card.
- so: SONET/SDH interface.
- sp: Adaptive services interface. The logical interface sp-fpc/pic/port.16383 is an internally generated, nonconfigurable interface for router control traffic.
- stm1: STM1 interface (configured on the OC3/STM1 interfaces).
- stm4: STM4 interface (configured on the OC12/STM4 interfaces).
- stm16: STM16 interface (configured on the OC48/STM16 interfaces).
- t1: T1 interface (including channelized DS3-to-DS1 interfaces).
- t3: T3 interface (including channelized OC12-to-DS3 interfaces).
- tap: Internally generated interface that is not configurable.
- umd: USB modem interface.
- vsp: Voice services interface.
- vc4: Virtually concatenated interface.
- vt: Virtual loopback tunnel interface.
- xe: 10-Gigabit Ethernet interface. Some older 10-Gigabit Ethernet interfaces use the ge media type (rather than xe) to identify the physical part of the network device.
- xt: Logical interface for Protected System Domains to establish a Layer 2 tunnel connection.