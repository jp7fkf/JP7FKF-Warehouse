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