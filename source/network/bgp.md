# BGP

## RFC
- [RFC 4271 - A Border Gateway Protocol 4 (BGP-4)](https://tools.ietf.org/html/rfc4271)

## BGPのtips

### iBGPはスプリットホライズンが効いている．
- iBGPは受信した経路広告を他のiBGPピアには広告しない  
  →ルーティングループ防止のため  
  →だからフルメッシュを張る必要がある

### BGP のAttibutes and Preference
```
1 Weight
2 Local Preference
3 Originate
4 AS path length
5 Origin code
6 MED
7 eBGP path over iBGP path
8 Shortest IGP path to BGP next hop
9 Oldest path
10  Router ID
11  Neighbor IP address

1 NEXT_HOP経路比較を行う前に、NEXT_HOP属性にて経路の到達性を確認します。ネクストホップに到達出来ない経路は無効経路となります。
2 WEIGHTCisco独自の属性。WEIGHT値が大きい値の経路が優先されます。
3 LOCAL_PREFWEIGHTが同じ場合は、LOCAL_PREF値を比較し値の大きい経路が優先されます。
4 LOCALローカル（自ルータ）で生成された経路が優先されます。
5 AS_PATHLOCAL_PREFが同じ場合は、ASパスを比較して短い経路が優先されます。
6 ORIGINAS_PATHが同じ場合は、ORIGINを比較して IGP＞EGP＞Incompleteの順に優先されます。
7 MEDORIGINが同じ場合は、MED値の小さい経路が優先されます。ただし同じAS内の隣接ルータから伝達されたMED値のみ比較します。同じ宛先の経路情報でも、異なるASから伝達されたMED値は比較されません。
8 PEER_TYPEMEDが同じ場合は、iBGP経路から受信した経路より、eBGP経路から受信した経路が優先される
9 IGP METRICPEER_TYPEが同じ場合は、ネクストホップに到達するまでのIGPのコストが小さい経路を優先します
10  ROUTER-IDすべてが同じであれば小さいルータIDのピアから受信した経路が優先されます

type code attr.name category
1 ORIGIN  Well-known mandatory
2 AS_PATH Well-known mandatory
3 NEXT_HOP  Well-known mandatory
4 MULTI_EXIT_DISC Optional non-transitive
5 LOCAL_PREF  Well-known discretionary
6 ATOMIC_AGGREGATE  Well-known discretionary
7 AGGREGATOR  Optional transitive
8 COMMUNITY Optional transitive
9 ORIGINATOR_ID Optional non-transitive
10  CLUSTER_LIST  Optional non-transitive
- WEIGHT  

カテゴリー 説明
　Well-known mandatory（ 既知必須 ） 　全てのBGPルータで識別できて、全てのUpdateメッセージに含まれる
　Well-known discretinary（ 既知任意 ）　 　全てのBGPルータで識別できるが、Updateメッセージに含まれるかは任意
　Optional transitive（ オプション通知 ） 　全てのBGPルータで識別できない可能性があるが、BGPネイバーへは通知する
　Optional non-transitive（オプション非通知）  　全てのBGPルータで識別できない可能性があり、BGPネイバーへは通知しない
```
- ref: [RFC 4271 - A Border Gateway Protocol 4 (BGP-4)](https://tools.ietf.org/html/rfc4271)
```
5.  Path Attributes

 This section discusses the path attributes of the UPDATE message.

 Path attributes fall into four separate categories:

       1. Well-known mandatory.
       2. Well-known discretionary.
       3. Optional transitive.
       4. Optional non-transitive.
```

## BGPってベストパスしかIBGPに入れないんだよね？バックアップパスは？
- バックアップパスを広告する方法は存在する．
  - BGP Advertise Best External
  - BGP add-path
  - BGP Diverse Path

## LoでBGPを張る
- [Sample Configuration for iBGP and eBGP With or Without a Loopback Address - Cisco](https://webcache.googleusercontent.com/search?q=cache:Z0j64jPkSo8J:https://www.cisco.com/c/en/us/support/docs/ip/border-gateway-protocol-bgp/13751-23.html+&cd=11&hl=en&ct=clnk&gl=jp)
- loを用いてebgp neighbor を構成するときにebgp-multihopは本当に必要か？
  - [Solved: ebgp multihop command only applies to l... - Cisco Community](https://community.cisco.com/t5/routing/ebgp-multihop-command-only-applies-to-loopbacks/td-p/2030331)

## private as num
- [RFC6996](https://tools.ietf.org/html/rfc6996)
  - 16bit/2bytes ASN
    - range: 64512 - 65534
    - 1023 ASNs
  - 32bit/4bytes ASN
    - range: 4200000000 - 4294967294
    - 94,967,295 ASNs
