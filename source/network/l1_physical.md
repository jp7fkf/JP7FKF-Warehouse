# L1 - Physical Layer
## 物理層の話 ## 2018-09-04 09:01:41 YudaiHashimoto
  - Gigabit Ethernetは4D-PAM5という変調方式を取っている．
    - 0レベルの他に上下2レベルの信号レベルがある
    → 4-dimentional - 5-level Pulse Amplitude Modulationかと．  
    → 4Dは4-wireの意  
    → 1-wireあたり250Mbps(125Mbaud), 62.5MHzのレート  
    → シリアルの1Gbps信号を4-wireにパラレル変換している．  
    →レベル0 はForwaed Error Ditectionにのみ用いられて信号伝送には使わない  
    - 8B1Q4でシンボルを生成して4D-PAM5で送る感じ

## 1000base-tと1000base-txの違い
- UTP4Pケーブルは中に線の1ペアが4つ（8芯）ある．
- 1000Base-Tでは、1ペアが各々250Mの双方向（行って来い）の送信、受信を1ペアの中で行います。250Mの双方向で送受信×4ペアで1000Mと言う事。
- 1000Base-TXは、1ペアで各々500Mの片方向の送信のみ、受信のみを行います。500Mの送信のみ×2ペアで1000Mと500Mの受信のみ×2ペアで1000Mと言う事。
- https://lan-cables.com/txtigai.html

- [1000BASE-TX か、1000BASE-T か：ネットワークエンジニアになろう！](http://www.smartnetworks.jp/2006/02/1000basetx_1000baset.html)
  - IEEE802.3ab(1000BASE-T)
    - ハイブリッド回路（方向性結合き的な）で送受信信号の分離している．
    - 4ペア間の同期を取るための処理も必要．
  ```
            +--- TX/RX ---- <-- 250Mbps --> ---- TX/RX ---+
            +--- TX/RX ---- <-- 250Mbps --> ---- TX/RX ---+
       <<---+                                             +--->>
            +--- TX/RX ---- <-- 250Mbps --> ---- TX/RX ---+
            +--- TX/RX ---- <-- 250Mbps --> ---- TX/RX ---+
  - EIA/TIA-854-A(1000BASE-TX)
    - ペア間同期とか，信号分離がいらないので安価．
    - Cheaper Gigabit Ethernet とも呼ばれる．

            +--- TX --------> 500Mbps --> --------- RX ---+
            +--- TX --------> 500Mbps --> --------- RX ---+
       <<---+                                             +--->>
            +--- RX ----- <-- 500Mbps <------------ TX ---+
            +--- RX ----- <-- 500Mbps <------------ TX ---+
  ```


## イーサネットの伝送方式まとめ # 2018-08-24 17:08:02 YudaiHashimoto
- 1Gbps
  - 光ファイバー
    - 1000BASE-SX：マルチモードケーブル （550m）
    - 1000BASE-LX：マルチモードケーブル（500m前後）、シングルモードケーブル（5 - 10km前後）
  - メタルケーブル
    - 1000BASE-T：（100m）

- 10Gbps
  - 光ファイバー
    - 10GBASE-SR：マルチモード光ケーブル （300m）
    - 10GBASE-LR：シングルモード光ケーブル （10km）
  - メタルケーブル
・10GBASE-T：（100m）

- ダイレクトアタッチは10m以下．実質ラック内配線用．
- シングルモードの光ファイバは略称SMF(Single Mode Fiber)といい、光を伝送するモードフィールド径が約9μm(7.1μm, 8.5μm)
- マルチモードファイバはMMF(Multi Mode Fiber)といって、コア径は約50μmまたは62.5μm
- クラッド径はいずも125μm

## SFP (Small Form-Factor Pluggable)
  - GBIC(Gigabit Interface Converter)の半分くらいのサイズであることからMini-GBICと呼ばれる．
  - SFP electrical pin-out
```
+----+------+-----------+ 
| Pin | Name | Function |
+----+------+-----------+ 
| 1 | VeeT | Transmitter ground |
+----+------+-----------+ 
| 2 | TxFault | Transmitter fault indication |
+----+------+-----------+ 
| 3 | TxDisable | Optical output disabled when high |
+----+------+-----------+ 
| 4 | MOD-DEF(2) | Data for serial ID interface |
+----+------+-----------+ 
| 5 | MOD-DEF(1) | Clock for serial ID interface |
+----+------+-----------+ 
| 6 | MOD-DEF(0) | Grounded by the module to indicate module presence |
+----+------+-----------+ 
| 7 | RateSelect | Low selects reduced bandwidth |
+----+------+-----------+ 
| 8 | LOS | When high, indicates received optical power below worst-case receiver sensitivity |
+----+------+-----------+ 
| 9 | VeeR | Receiver ground |
+----+------+-----------+ 
| 10 | VeeR | Receiver ground |
+----+------+-----------+ 
| 11 | VeeR | Receiver ground |
+----+------+-----------+ 
| 12 | RD- | Inverted received data |
+----+------+-----------+ 
| 13 | RD+ | Received data |
+----+------+-----------+ 
| 14 | VeeR | Receiver ground |
+----+------+-----------+ 
| 15 | VccR | Receiver power (3.3 V, max. 300 mA) |
+----+------+-----------+ 
| 16 | VccT | Transmitter power (3.3 V, max. 300 mA) |
+----+------+-----------+ 
| 17 | VeeT | Transmitter ground |
+----+------+-----------+ 
| 18 | TD+ | Transmit data |
+----+------+-----------+ 
| 19 | TD- | Inverted transmit data |
+----+------+-----------+ 
| 20 | VeeT | Transmitter ground |
+----+------+-----------+ 
```

##  クロストーク
- 近端クロストーク（Near-End Crosstalk, NEXT）
- パワーサム近端クロストーク（Power Sum NEXT, PS-NEXT）
- 遠端クロストーク（Far-End Crosstalk, FEXT）
- 電力和等レベル遠端漏話（Power Sum Equal Level FEXT, PS-ELFEXT）

## SFPの規格ってどないなっとんねん
  - SFF-8431 Specifications for Enhanced Small Form Factor Pluggable Module SFP+
  - https://www.snia.org/technology-communities/sff/specifications
  - https://www.snia.org/sff
  - http://www.10gtek.com/templates/wzten/pdf/INF-8074.pdf

## MTPコネクタ
  - MPOコネクタの進化版らしい．
  - MTPはUS Conecの登録商標とのこと．
  - [MTP コネクタ](https://www.empowerfiber.com/ja/mtp-connector.html)
  - [MTPコネクタとMPOコネクタ：違いは何ですか？ - sunny3210’s blog](https://sunny3210.hatenablog.com/entry/2018/10/06/152232)

## FlexULC
  - [FlexULC™付２心ラウンドコード - 製品情報 | 住友電工Optigate](http://www.optigate.jp/products/connector/mpo/2-fulc.html)
  - ファイバ2芯分が1本の外部被覆にくるまっている．
  - スペース的にも1/2になるみたい．配線がかさばることが想定されるところに適用するとよさそう．