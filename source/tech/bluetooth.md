# Bluetooth

## BLEとBluetooth Classic

## BLE
- セントラルとペリフェラル
  - ペリフェラルはadvertise
  - connect requrestを送るのはcentral
  - GATTサーバ/クライアントとの関係(Attribute Protocol(ATT)
    下記のように対応に制約はない
    - ペリフェラルがサーバーであり、セントラルがクライアント(一般的)
    - クライアントのみのペリフェラル
    - サーバーとクライアントの両方のセントラル

## 階層
- Host層
  - Attribute Protocol(ATT)
    - method
      - Request: クライアントからサーバにデータを要求する
      - Response: Requestに対する応答
      - Command: クライアントからサーバに命令を送る。返答は要求しない
      - Notification: サーバからクライアントに通知を送る。受け取りの確認は必要ない
      - Indication: サーバからクライアントに通知を送る。受け取りの確認が必要
      - Confitmation: Indicationを受け取ったことの確認をクライアントからサーバに送信する
    - Request/ResponseとIndication/Confitmationはそれぞれ対でトランザクション
- Generic Access Profile(GAP)
- Generic Attribute Profile(GATT)

## [Bluetooth Advertising Data Basics - v4.0 - Bluetooth API Documentation Silicon Labs](https://docs.silabs.com/bluetooth/4.0/general/adv-and-scanning/bluetooth-adv-data-basics)
