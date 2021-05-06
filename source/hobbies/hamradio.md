# Amateur Radio

## 1.9-430MHzのうちバンドプランで許可されている型式
### 広帯域データ
28MHz
50MHz
144MHz
430MHz

### 広帯域画像
28MHz
50MHz
144MHz
430MHz

### 狭帯域データ
1.9MHz (占有周波数帯幅200Hz以下に限る)
3.5MHz
7MHz
10MHz
14MHz
18MHz
21MHz
24MHz
28MHz
50MHz
144MHz
430MHz

### 画像，映像
3.5MHz
3.8MHz
7MHz
14MHz
18MHz
21MHz
24MHz
28MHz
50MHz
144MHz
430MHz

## RTTY
- 諸元
  - 方式: AFSK/FSK
  - 通信速度: 45～110ボー
  - 副搬送波周波数 500Hz～2,210Hz
  - 周波数偏移幅: 170Hz
  - 符号構成: BAUDOT
  - 電波型式: F1B,F2B
- memo
  - 周波数繊維幅は副搬送波周波数±偏移周波数なので仮に85Hzの繊維周波数だとしたら周波数繊維幅は170Hz

- ex1.
```
RTTY
(注 1)
方式 AFSK,FSK 第 1 装置
通信速度 RTTY/GMSK/FSK:20～250bps MFSK:15.625～31.25bps
副搬送波周波数 500～2,210Hz (A2B 時 500～1,000Hz)
周波数偏移幅
RTTY:±42.5/85/170Hz FSK:±85Hz GMSK:±62.5Hz
MFSK:±117.1875Hz (最大ｷｬﾘｱ数：16 ｷｬﾘｱ間隔：±15.625Hz)
符号構成 RTTY:BAUDOT FSK/GMSK:VARICODE JA MFSK:MFSK-VARICODE
装置出力最高周波数 AFSK:2,310Hz (A2B 時 1,000Hz)
電波型式 SSB/F1B FM/F2B AM/A2B (注 2)
```

- ex2. [諸元表](http://www.maroon.dti.ne.jp/k3is/syogenh.htm)
```
RTTY 方式  AFSK/FSK　
通信速度  45～110ボー
副搬送波周波数 500Hz～2,210Hz（但しA2Bのみ500～1,200Hz）
周波数偏移幅  ±170Hz　
符号構成  BAUDOT　
電波型式  F1B,A2B,F2B
```
- ex3. https://blogs.yahoo.co.jp/je1ngi599/GALLERY/show_image.html?id=10185504&no=0

## 東北総合通信局に書いてあること
```
■東北総合通信局へ直接申請する変更内容（保証が不要）
すでに許可を受けている無線機に附属装置（RTTY・ハﾟケット・SSTV等）を追加する場合
技適機種の増設・買替えの場合（※）　ただし、新スプリアス規格の技適機種に限ります。
　※同一の変更申請で附属装置（RTTY・ハﾟケット・SSTV等）を追加する場合は保証が必要となります。
空中線電力200Wを超える無線設備等の変更申請。この場合、変更内容によっては「変更検査」の受検が必要となる場合があります。
```
- ref. http://www.soumu.go.jp/soutsu/tohoku/tetuduki/amateur/index.html

## 代表的なソフトウェアってこんなかんじ？
RTTY装置 → MMTTY
FSK装置及びPSK装置 → MMVARI
アナログ画像装置 → MMSSTV
デジタル画像装置 → EasyPal
WSPR装置 → WSPR
JT65A/B/C装置 → WSJT, JT65-HF
FSK441装置 → WSJT

## SSTV

### デジタルSSTV
- 一昔前は DigTRXが使われていた．RDFT方式と呼ばれる変調方式を採用．作者のPCがクラッシュして開発中止になったらしい．ウケる．
- いまは，HamPAL/EasyPALが主流になっている．変調方式はDRM (Digital Radio Mondiale)．
- ref. http://www.ne.jp/asahi/jh1htq/machida/sstv/digital/digital.html

## misc
- [アンテナケーブル、他 / 横浜システムマリン](http://www.ysmweb.co.jp/mail_order/sub_12.html)
- マグネチックループ造りたい．shielded magnetic loopがよさそう．ポータブル，マルチマッチングにできないか．
