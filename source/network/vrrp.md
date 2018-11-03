# VRRP
  - Virtual Router Redunduncy Protocol
  - RFC3768
  - デフォルトゲートウェイ(ファーストホップルータ: FHR)を冗長化．
  - 基本的にdefault gatewayは1つのIP addressしか書かないため．

## 仮想mac
  - 00:00:5e:00:01:<vrid> 的なvirtual mac address がつく．

## Advertisement
  - Interval は default で 1sec?
  - VRRPを有効にしたルータ 間でVRRP情報をやり取りするマルチキャスト
  - Multicast Address: 224.0.0.18 
  - IP Protocol Number: 112

## Preempt
  - 無効だと，Backup はMasterがVRRP Adv. を送信している限り，PriorityによらずAdv. しない．
  - 有効だと，自身のPriorityを比較し，自身のPriorityが大きい場合のみAdv.する．
  - つまるところ，Preemptが有効じゃないと，configでpriority変えても即座に反映されないことがあるということ．

## Master Down Interval
  - Master_Adv_Interval * 3 + (256 - Priority)/256
  - Skew Time(IPv4): (256 - priority)/256; part of above eq.
  - Skew Time(IPv6): ((256 - priority) * Master_Adv_Interval) / 256