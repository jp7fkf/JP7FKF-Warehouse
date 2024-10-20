# bluetooth serial adapter

## specs
- input
  - bluetooth serial
  - button/encoder
- output
  - LCD out
  - RJ-45(RS232 level) cisco pin assign serial out

## power source
- USB-C chargeable builtin battery

## functions
- multi-baud rate
  - set in device interface or special serial commands

## parts
- main MCU
  - ESP-32
- RS232 interface 
  - [RS232CインターフェイスIC ICL3232CVZ: 半導体 秋月電子通商-電子部品・ネット通販](https://akizukidenshi.com/catalog/g/g102375/)
- RJ45 connector
  - [MTJ-88ARX1-LH Adam Tech | コネクタ、相互接続 | DigiKey](https://www.digikey.jp/ja/products/detail/adam-tech/MTJ-88ARX1-LH/9832414?s=N4IgTCBcDaILIBUBSBaAHGgggJQBoEYUAZACRAF0BfIA)
  - LEDをtx/rx indicatorとして利用する
