# Making

## ハードウェア
### 天体望遠鏡

### アンテナアナライザ
- 基本的に下記の記事を参考にさせてもらう．本当によくまとめられていて大変ありがたい．
  - [アンテナアナライザの回路 シリーズ抵抗1本型 | tech - 氾濫原](https://lowreal.net/2016/03/01/1)
  - [アンテナアナライザの回路 ブリッジ型 | tech - 氾濫原](https://lowreal.net/2016/03/01/2)
  - [アンテナアナライザの回路 ー 位相検出器を使ったタイプ | tech - 氾濫原](https://lowreal.net/2016/03/04/1)
  - [アンテナアナライザの回路 - ブリッジの三つの電位差を測るタイプ | tech - 氾濫原](https://lowreal.net/2016/03/03/1)
  - [アンテナアナライザーの回路 ー まとめ | tech - 氾濫原](https://lowreal.net/2016/03/06/1)
  - [簡単かつ安く高精度なアンテナアナライザーを自作したい | tech - 氾濫原](https://lowreal.net/2016/03/20/1)
  - [簡単かつ安く高精度なアンテナアナライザーを自作したい (2) | tech - 氾濫原](https://lowreal.net/2016/03/22/1)
  - [アンテナアナライザーをケースに収めた | tech - 氾濫原](https://lowreal.net/2016/04/01/1)
  - [自作アンテナアナライザーのBluetooth化とアプリケーション | tech - 氾濫原](https://lowreal.net/2016/03/26/1)

- AD8307ログアンプで電力測定
- 発振器は AD9851 DDSモジュールを利用
- MCUはSTM32を利用しようか．
- Bluetooth 搭載．
- RFアンプはカジュアルにNXPのMMG3H21NT1を使おうかと思っている．

- 憶測だが，アンテナを電波暗室においてない以上，環境から受電するエネルギは避けられないので，測定時には入ってくると考えられる．
- その影響を極力排除するには，測定時に投入するエネルギをやや強めにして環境由来のエネルギとの差を大きくする以外の手法がない気がする．
- 受電電力がuWオーダだとして，数mW程度の入力ならば問題ないのではなかろうかと考える．これは市販のものがどれくらいのエネルギを測定時に出しているのか知りたい．
- 測定時にノイズを減らすには，アンテナからん反射があろうとなかろうと，測定用電力が著しく小さくならないように留意する必要性がある気がする．
- RFアンプの出力インピーダンスと接続先（アンテナ等）のインピーダンスがずれすぎて，RFアンプからの出力エネルギーが小さくなりすぎると，RFスイッチングして測定するにしても，エネルギー差が生まれにくい（ノイズに埋もれる）可能性が大変高い．これは不安定な計測結果を得てしまう原因になりうる．

### 電子負荷装置
- 制御対象: 定抵抗，定電流，定電力，（定電圧？）制御
- DAC出力でオペアンプに指令電圧を印加
- オペアンプはエラーアンプになっており，指令電圧とフィードバック電圧の差を0にするようにFETのゲートに対して電圧を出力．
- オペアンプとFETゲートの間にはバッファTrを入れる
  - ゲートチャージに対応．
  - 測定対象の電源装置の過渡応答特性の測定等を行うため，急峻なステップ応答特性を出したい．
- サイクルステップモード（過渡応答特性測定モード）を用意したい．
  - n秒ごとにステップ応答となるようにFETの抵抗値を制御．
  - オシロで測定対象の電源電圧，電流を測定することで電源の特性を見る．

- 必要ピン数:
- MCUから:
  - I2C for DAC: x2
  - LCD signal: x7
  - UI系: ロタコンx2, enableスイッチx1, select用スイッチx1?
  - 温度センサx1(ADC)
  - ファン制御PWMx1
  - ロガー用serial out
  - 電圧，電流監視(電流は足し算でいいとして．．．)x1pin電圧用？

- 基板から
  - 4パラFET構成．(司令電圧x1 + 電流センス差動x2) x4
  - LCD

## ヘッドホンアンプ
  - [プアオーディオはじめました。～ BEHRINGER HA400 がやってきた～ 色白腹黒の気まぐれ帳/ウェブリブログ](https://irojiro-haraguro.at.webry.info/201406/article_1.html)
  - [メモ帳:BEHRINGER MICROAMP HA400 - livedoor Blog（ブログ）](http://blog.livedoor.jp/r_ten/archives/51933905.html)
  - [ベリンガーヘッドフォンアンプHA400のオーディオ化改造](https://skeishi.web.fc2.com/audio/kaizou/HA400/HA400.html)
  - [HA400改造についての注意点：KAZNYANの備忘録](https://s.webry.info/sp/kunekune.at.webry.info/200903/article_5.html)

### やっぱbt console需要あると
- RN4020では無理．BT Classis使う．

#### etc
- tdr 、じっくりとCCCV電源、信号処理

## ソフトウェア

### cw freak web ver
- があるとよさそう

### モニタリングからのトリガであるシナリオを実施するくん．下記のような
- インテントベースのネットワーク
  - とはいえ，最初は自動復旧するようなやつがほしい．
  - モニタリングと，それをトリガして一定のシナリオの復旧操作をする．この復旧操作には人間の承認があってもいい．
  - 復旧操作がfailしたら人間にアラートする．人間がなんとかする．この人間がなんとかしないといけない部分を↑のシナリオにもっていけるようにすると人間がやらないといけないことは減る．
  - このサイクルを回すと自ずと自律的なシステムに近づく．スマートではないが，レガシーシステムにも適用できうる．

### oxidized改

### deadman改

## tips

### Replicator 2x
- Shuhei Udaさんのところがよくまとめられていて大変参考になる
  - [Replicator 2X 日本語マニュアル – Made in container](https://www.syuheiuda.com/?p=1775)
  - [“replicator” の検索結果 – Made in container](https://www.syuheiuda.com/?s=replicator)
- MakerBot MakerWare は201911現在discon.
- 201911現在，softwareとしてはMakerbot Printが存在するが，これだとdual ext.が使えない．
  - dual ext. 出力したい場合は，MakerBot Desktopを使うといい．
  - [Installing MakerBot Desktop | MakerBot Desktop (Software) | MakerBot Support](https://support.makerbot.com/learn/makerbot-desktop-software/installation-and-setup/installing-makerbot-desktop_11222)
  - 無論2xも対応している．deviceから選択する．
- PLAはノズルがつまりやすい．原因はまだ不明だがシリコンオイルを塗布してやると改善するよう．シリコンスプレーでいい．
  - print中にいきなりエクストルーダがガチガチ言い出して樹脂を吐けなくなる．Fillament Unload/Loadしないと治らない．
- Catalina からはMakerBot Desktopが動かなくなった．かなしい．Windows10ではまだ動くのでそっちでコード生成してる．
- [How to Unclog a Nozzle | MakerBot Replicator 2X | MakerBot Support](https://support.makerbot.com/learn/makerbot-replicator-2x/extruders/how-to-unclog-a-nozzle_13469)
- extは右が0, 左が1
- ext間隔は35mm

### 3D Printingの条件
すくなくとも手持ちのReplicator 2xで良さげだと思われるパラメータを記載する．
- ABS:: head:230℃, heatbed: 110℃
- PLA:: head:220℃, heatbed: 60℃ # わりとやわやわ
- 積層ピッチは0.2mmがきれいでよい

- サポートの条件
```eval_rst
.. image:: ../resources/images/3dprint_setting_01.png
```

## firmとか
- [ReplicatorG is a simple, open source 3D printing program - ReplicatorG](http://replicat.org/)
  - [Google Code Archive - Long-term storage for Google Code Project Hosting.](https://code.google.com/archive/p/replicatorg/downloads)
- [SCons: A software construction tool - SCons](https://scons.org/)
- [GitHub - markwal/GPX: Gcode to x3g conversion post processor](https://github.com/markwal/GPX)
- [GitHub - jetty840/Sailfish-MightyBoardFirmware: Sailfish, faster than a Marlin](https://github.com/jetty840/Sailfish-MightyBoardFirmware)
- [Sailfish Firmware by jetty - Thingiverse](https://www.thingiverse.com/thing:32084)

## replicatorGでサポートしてるGコード/Mコード
- [G Codes - ReplicatorG](http://replicat.org/gcodes)
- [M Codes - ReplicatorG](http://replicat.org/mcodes)
- [G-code - RepRap](https://reprap.org/wiki/G-code)

## 3d bency
- No.1:
  - firmwarereplicator2x 7.6
    - [Support Community](https://support.makerbot.com/s/article/Replicator-2X-Firmware)
  - 吸湿してそうなフィラメント
  - 温度230/110
- No.2: diffだけかいていく
  - 吸湿してなそうなフィラメント
- No.3:
  - No.1のフィラメント
  - sailfish for replicator2x
    - Sailfish 7.7 (r1432)
- No.4
  - prusa slicerでreplicator 2x rev0.1
  - infill 20%
  - layer 0.3mm
- No.5
  - prusa slicerでreplicator 2x rev0.1
  - infill 10%
  - layer 0.2mm

## max speed
- feed: 120mm/s
- travel: 150mm/s

## フィラメント在庫
- PLA 1.75mm 透明(natural) MakerBot
- ABS 1.75mm Green 謎メーカー
- PLA 1.75mm 透明(natural) MakerBot
- PVA 1.75mm WATER SOLUBLE MakerBot
- PLA 1.75mm TrueRed MakerBot
- [Amazon.co.jp: OVERTURE ABS フィラメント 3Dプリンター用素材 3D印刷 3Dプリンター フィラメント 1.75mm 1kgスプール 高い寸法精度 高密度 環境保護 ほとんどのFDMプリンターに適合 (ブラック) : 産業・研究開発用品](https://www.amazon.co.jp/gp/product/B08LNCPR9F/)
  - ABS 1.75mm Black
- [Amazon.co.jp: OVERTURE PETG フィラメント PETG 3Dプリンター用素材 3D印刷 3Dプリンター フィラメント 1.75mm 1kgスプール 高い寸法精度 高密度 環境保護 ほとんどのFDMプリンターに適合 (ブラック) : 産業・研究開発用品](https://www.amazon.co.jp/gp/product/B08LNBS1F8/)
  - PETG 1.75mm Black