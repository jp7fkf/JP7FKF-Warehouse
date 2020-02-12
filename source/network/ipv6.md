# IPv6

## MacでIPv6の有効/無効
- sudo networksetup (-setv6off | -setv6automatic) <networkservice(ex.Wi-Fi)>

## RAのM/O-Flag
```eval_rst
.. list-table:: IPv4 RA Flags
    :header-rows: 1
    :widths: 5, 5, 5

    * - M-Flag
      - O-Flag
      - Info
    * - 0
      - 0
      - | RAのみを利用したステートレス自動設定
        | IPv6アドレスやGWはAからの情報に基づいて自身で生成
        | IPv6アドレス以外のパラメータ（DNS等）は手動で設定
        | ※RFC6106に対応していれば、DNSサーバのIPもRAで配布可能
    * - 0
      - 1
      - | DHCPv6サーバを使用したステートレス自動設定
        | IPv6アドレスやGWは、RAからの情報に基づいて自身で生成する
        | IPv6アドレス以外のパラメータ（DNS等）はDHCPv6サーバから取得

    * - 1
      - 0
      - IPv6アドレスはDHCPv6サーバから取得し他のパラメータは手動設定

    * - 1
      - 1
      - | DHCPv6サーバを使用したステートフル自動設定
        | DHCPv6サーバからIPv6アドレス、GW、DNSなどを取得できる
        | DHCPv6サーバでクライアントに割り当てたIPv6アドレスを管理
        | ※現時点でDHCPv6でデフォルトゲートウェイ情報を配布できない
```
- ref.) https://www.infraexpert.com/study/ipv6z5.html

## IPv6 アドレス
### 種類
- Unicast
  - Global Unicast
    - `2001::/16`: IPv6インターネット用アドレス
    - `2002::/16`: 6to4用アドレス
    - `2003::/16` ～ `3FFD::/16`: 未割り当て
  - Unique Local
    - `FC00::/8`: for the Future
    - `FD00::/8`: アドレス中のグローバル識別子部分をランダムな値としていつでも誰でも利用可能なアドレス
  - Lonk Local
    - `FE80::/64`
- Multicast
  - `FF02::1`: リンクローカルノードマルチキャスト
  - `FF02::2`: リンクローカルルータマルチキャスト
  - `FF02::1:FFxx:xxxx`: 要請ノードマルチキャスト
    - `xx:xxxx`はMAC解決をしたいIPv6アドレスの下位3bytes
  - `FF02::5`: リンクローカルOSPFv3ルータ
  - `FF02::6`: リンクローカルOSPFv3 DR/BDRルータ
  - `FF02::9`: リンクローカルRIPngルータ
  - `FF02::A`: リンクローカルEIGRPルータ
  - `FF02::D`: リンクローカルPIMルータ
  - `FF02::1:2`: リンクローカルDHCPエージェント
  - `FF05::1:3`: サイトローカルDHCPサーバ
  - `FF02::101`: リンクローカルNTPサーバ
  - `FF05::101`: サイトローカルNTPサーバ
- Anycast
- 特殊アドレス
  - :`:1/128`: Loopback

※ipv6にはbroadcastアドレスは存在しない．
代わりに`ff02::1`などのlinklocal all-node multicastなどが存在する．

### Scope
- Link-Local
- Unique-Local
- Global

### EUI-64によるInterface ID
- 48bit mac(`11:22:33:44:55:66`)を24bitずつに分け，中央にFFFEを挿入
  - `11:22:33:FF:FE:44:55:66`
- 7bit目をbit flip
  - (完成)Int-ID: `13:22:33:FF:FE:44:55:66`

## ICMPv6

### type
- Error
  - 1: Destination Unreachable （ 宛先到達不能 ）
  - 2: Packet too Big （ パケット過大 ）
  - 3: Time Exceeded （ 時間切れ ）
  - 4: Parameter Problem （ パラメータ異常 ）
- Info
  - 128: Echo Request （ エコー要求 ）
  - 129: Echo Reply （ エコー応答 ）
  - 130: Multicast Listener Query （ マルチキャストリスナクエリー ）
  - 131: Multicast Listener Report （ マルチキャストリスナレポート ）
  - 132: Multicast Listener Done （ マルチキャストリスナダン ）
- Neighbor Discovery
  - 133: Router Solicitation （ ルータ要請 ）
  - 134: Router Advertisement （ ルータ広告 ）
  - 135: Neighbor Solicitation （ 近隣要請 ）
  - 136: Neighbor Advertisement （ 近隣広告 ）
  - 137: Redirect （ リダイレクト ）

### 近接探索を用いたMACアドレス解決
1. A --NS--> B : AからBへ要請ノードマルチキャストで でNeighbor Soliciationを投げる
  - `FF02::1:FFxx:xxxx`: 要請ノードマルチキャスト
    - `xx:xxxx`はMAC解決をしたいIPv6アドレスの下位3bytes
1. A <--NA-- B : BからAへunicastでNeighbor Advertisementを投げる