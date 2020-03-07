# Raspberry pi

## モデルとか確認する
```
## version 確認
$ lsb_release -a

## kernel 確認
$ uname -a

## raspberry piハードウェアのモデルを確認する方法
$ cat /proc/device-tree/model
```

## おまじない
-  [Raspberry PiのSDカードが壊れた！寿命を延ばす方法 5+1選!【運用編を追加】 | IoT PLUS](https://iot-plus.net/make/raspi/extend-sdcard-lifetime-5plus1/)

## GNSS Stratum1 NTP

### References
- [Raspberry Pi + GPS ModuleでStratum1なNTPサーバをつくる](raspintp.md)
- [#2 Raspberry Piで作るStratum1 NTPサーバをベランダで動かすまで - 猫にジャズ](https://notchi.hatenablog.jp/entry/2019/05/06/165659)
- [NMEA 0183 sentences データ解析 – piyajk.com](https://piyajk.com/archives/302)
- [GitHub - MartijnBraam/gpsd-py3: Python 3 GPSD client](https://github.com/MartijnBraam/gpsd-py3)

### きのこアンテナ
- HP Symmetricom 58532A
- [58532A | eBay](https://www.ebay.com/sch/i.html?_from=R40&_trksid=m570.l1313&_nkw=58532A&_sacat=0&LH_TitleDesc=0&_osacat=0&_odkw=symmetricom+58532A&LH_TitleDesc=0)