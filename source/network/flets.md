# What is the Flet's

## フレッツ/Flet'sとは
- Flat/Friendly/Flexible + Let's
- 当初は地域IP網(都道府県内通信)として電話交換局間の通信に利用されていた．
- 各都道府県のPOIにてISPと接続．長距離バックボーンによる相互通信．
- 光コラボ(2015/02)によりフレッツ回線を解放し，多くの事業者が電気情報通信事業に参画．
- NGN化．IPv6なnetwork．
- VNEによるインターネット接続性の提供

## ONUとは
- Optical Network Unit
  - ONUはIEEE 802.3ahで標準化されており高い互換性をもつ．
  - ONUに対して，通信事業者側にはOLT(Optical Line Terminal)という終端装置が存在する．
  - ONUには他のONUと(少なくともそのGE-PONドメイン配下では)バッティングしないIDが割り当てられている(LLID)．
  - 実際には許可されたONUのみが接続できる様認証が入っていたり，他のLLID向けの信号が読み取れないよう暗号化されていたりする．
- 小型ONU
  - [小型ONU開発・提供 | パートナー | 法人のお客さま | NTT東日本](https://business.ntt-east.co.jp/service/onu/)

## GE-PON(Gigabit Ethernet Passive Optical Network), IEEE802.3ah
- 方式
  - SS, Single Star: 局舎からファイバを伸ばす
  - ADS, Active Double Star: 局舎からのファイバ + 途中に多重化装置．多重化装置から宅内まではmetal．
  - PDS, Passive Double Star: 局舎からのファイバ + 光スプリッタ
    - 例えば戸建プランの場合，1Gbpsを光スプリッタで最大32分岐してTDMA的多重を行なっているものなどが存在する．
- 多重化
  - 下りはTDM(Time Division Multiplexing): OLT側での送信のみであるため．
    - ONUはLLID(Logical Link ID)が自らのものとmatchした場合，またブロードキャストLLIDの場合のみに転送する．
  - 上りはTDMA(Time Divition Multiple Access): ONU側からの衝突を防ぐためタイミング制御を実施．
    - 上り方向は自らのLLIDを送信フレームに埋め込んで送信．
    - 上り方向制御はOLTからの信号送信タイミングの指示(GATE frame)によって送出する．これによって衝突を防いでいる．
  - 上りと下りをWDM
    - 1000Base-PX10等で伝送される．

とどのつまりL2多重のための技術という感じ．

## MAP-E
- v6プラス
- v4 over v6技術
- NATステートレス
- 利用者側のIPv6 PrefixからIPv4，および利用可能なポートが計算される．
- それらのIPv4(GIP)，ポートでNATしてv6でencapして送信する．
- 当然だが利用者が利用できる外部ポートのレンジは計算されたものに限られるので一般に任意のポートを解放したりすることはできない．
- 計算されたポートレンジ内であれば可能である．

## DS-Lite
- transix
- v4 over v6技術
- NATステートフル
- v4のnatはトンネル終端側で実施
  - 1つのIPv4を複数の利用者で共有．CG-NATだと思えばいいか．
- global addrはトンネル始端では見えない．(たぶん)

## References
- GE-PON関連
  - https://www.ntt.co.jp/journal/0509/files/jn200509091.pdf
  - https://www.ntt.co.jp/journal/0508/files/jn200508071.pdf
  - https://www.ntt.co.jp/journal/0503/files/jn200503075.pdf
- DS-Lite
  - [RFC 6333 - Dual-Stack Lite Broadband Deployments Following IPv4 Exhaustion](https://tools.ietf.org/html/rfc6333)
- MAP-E
  - [RFC 7597 - Mapping of Address and Port with Encapsulation (MAP-E)](https://tools.ietf.org/html/rfc7597)
