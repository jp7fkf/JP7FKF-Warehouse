# Optical
## 規格

## ファイバをどこで買う？
- [Black Box](https://www.blackbox.co.jp/ja-jp/fi/1261/12164/PVC-625-m/)
- 愛三電機
- [fs.com](https://fs.com): アホほど安いけど品質がわからない．

## 光の分散ってなに
- 真空中では光速は一定．
- しかし一般に物質中では波長が短い(振動数が大きい)ものほど速度は遅くなる．
  - 理由は...? (難しいので調査がTODO)
  - ex): ガラスなどを構成する原子では、赤色波より紫色波の方がガラス構成原子の固有振動数に近く、波を吸収してから再放出するまでに時間が掛かる．（謎）
  - ref): 特に断らないときには、光学材料の屈折率は波長589.3 nmの光（ナトリウムのD線）について示す．(from: [屈折率 - Wikipedia](https://ja.wikipedia.org/wiki/%E5%B1%88%E6%8A%98%E7%8E%87))

## ファイバのJJの種類．
- リン青銅製スリーブ: やわらかい -> コアずれがおきやすそう．挿入時にスリーブの摩耗粉が出るらしい．MMF向き？
- ジルコニア製スリーブ: 硬くてコアずれしにくいらしい．SMFでも使えそう？
- とりあえずジルコニア使えということかもしれない．高いけど．
- ちなみにジルコニアは二酸化ジルコニウム(ZrO_2)のこと．人工ダイヤ的な．
- [ジルコニア - Wikipedia](https://ja.wikipedia.org/wiki/%E3%82%B8%E3%83%AB%E3%82%B3%E3%83%8B%E3%82%A2)
- Panduit 参考: 
    分割スリーブ材質: 
      ジルコニアセラミック: 10Gig™ OM3/OM4 マルチモードアダプタ, OS1/OS2 シングルモードアダプタ
      リン青銅: M1 および OM2 マルチモードアダプタ

## 伝送装置
- 100Gbps ROADMとSONET／SDH, L2 Aggregation
- 100Gbps×C band 88波（8.8Tbps）とかを束ねる
- デジタルコヒーレント
- DP-QPSK変調
- メトロコアから超長距離まで．
- つまるところAMP + WDM 的な．

- [光通信波長帯(T,O,E,S,C,L,U-バンド)とは | ファイバーラボ株式会社](https://www.fiberlabs.co.jp/column/wavelength/)
- 光にもバンド名がついている．
  ```
  Tバンド(Thousand-band)  1000 ~ 1260 nm
  Oバンド(Original-band)  1260 ~ 1360 nm
  Eバンド(Extended-band)  1360 ~ 1460 nm
  Sバンド(Short-wavelength-band)  1460 ~ 1530 nm
  Cバンド(Conventional-band)  1530 ~ 1565 nm
  Lバンド(Long-wavelength-band)   1565 ~ 1625 nm
  Uバンド(Ultralong-wavelengrh-band)  1625 ~ 1675 nm
  ```

## ファイバアンプ
- Oバンド-PDFA(Praseodymium Doped Fiber Amplifier)
- C・Lバンド-EDFA(Erbium Doped optical Fiber Amplifier)
- Sバンド-TDFA(Thulium Doped Fiber Amplifier)

## OM[1234]とか，SRとかERとかSXとか...そろそろちゃんとまとめないといかん．
- https://www.tsuko.co.jp/no31_qanda_tb1.html