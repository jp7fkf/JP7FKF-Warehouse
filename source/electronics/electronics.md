# Electronics

## Diode
### いい話
- 10mA想定時のForworVoltageがすごい
- If-Vfグラフを見てる

## 手持ちの部品
- LTC3119IDF
- LTC5551IUF
- LTC4358IFE
- LTC4447IGN
- ADG918BRMZ
- AD9834BRUZ
- AD7405BRIZ
- AD9834CRUZ
- ADP2291ARMZ
- ADF4350BCPZ
- HMC439S16GETR
- HMC439S16GE
- TCD-20-4X+ x4
- HMC1002 x1
- HMC1001 x1
- AD8362 x2
- AD8307 (ARZ x6) (ANZ x3)

## チップ部品の寸法
```
LxW(mm),  呼び:(mm, inch)
0.4mm × 0.2mm, 0402, 01005
0.6mm × 0.3mm, 0603, 0201
1.0mm × 0.5mm, 1005, 0402
1.6mm × 0.8mm, 1608, 0603
2.0mm × 1.2mm, 2012, 0805
3.2mm × 1.6mm, 3216, 1206
3.2mm × 2.5mm, 3225, 1210
5.0mm × 2.5mm, 5025, 2010
6.4mm × 3.2mm, 6432, 2512
```

## [Fetmet](https://www.muratasoftware.com/en/)

## 起こしたい基板候補
- イコライザ(slide volume)
- 電子負荷装置
- tandem SWR meter
- RF Power Meter?
- 電力監視
- FAST-mini
- matrix mixer
- ClassDamp?
- STM32/ESP board(omake
- nano keyer

## Projects
- 電子負荷装置proto
  - IO: ロタコン，ボタン系
- bluetooth serial, logger, bt multi, bt audio, proto
  - I2C, I2S, LiPo charge-discharge test
  - TiのI2S codec IC
  - 面実装レバースイッチ
  - USB-C
  - ICL3232CVZ
  - TRX ACT LED by P-FET
  - I2C 16x2 backlight LCD
- audio-master proto
  - YDA142 test, matrix switch test
  - JRCのマトリクススイッチIC: NJU7275
  - LED付きスイッチ
- raspi console server hat proto
  - USB HUB IC
  - CH340K
- majika iris hack proto
- SFP hacker
- rest api physical pad(esp-wroom-02)
