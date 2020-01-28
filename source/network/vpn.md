# VPN

## About VPN
- Encription vs Tunneling Protocol
```
+-------+----------+------------+
| Layer | Protocol | Encryption |
+-------+----------+------------+
|   L2  | PPTP     |     NO     |
|   L2  | L2TP     |     NO     |
|   L3  | GRE      |     NO     |
|   L3  | IP in IP |     NO     |
|   L3  | IPsec    |    YES     |
+-------+----------+------------+
```

## IPsec(IP Security)
- L2TP over IPsec
- GRE over IPsec
- IP in IP と IPsec
- Transparent mode
  - 暗号化範囲: TCP/UDPヘッダ - ESPトレイラ
  - 認証範囲: ESPヘッダ - ESPトレイラ
  - ex.)
  ```
  +-----------+--------+--------+------+---------+--------------+
  | Original  | ESP    | TCP    | Data | ESP     | ESP          |
  | IP Header | Header | Header |      | Trailer | Auth. Header |
  +-----------+--------+--------+------+---------+--------------+
  ```
- Tunnel Mode
  - 暗号化範囲: IPヘッダ - ESPトレイラ
  - 認証範囲: ESPヘッダ - ESPトレイラ
  - ex.)
  ```
  +-----------+--------+-----------+--------+------+---------+--------------+
  | NEW       | ESP    | Original  | TCP    | Data | ESP     | ESP          |
  | IP Header | Header | IP Header | Header |      | Trailer | Auth. Header |
  +-----------+--------+-----------+--------+------+---------+--------------+
  ```
- Protocols in IPsec
  ```
  +----------+---------------------------+----------+-------------+
  | Protocol | Usage                     |   Port   | IP Protocol |
  +----------+---------------------------+----------+-------------+
  | IKE      | KeyExchange               | 500(UDP) |      -      |
  | ESP      | Encryption/Authentication |     -    |     50      |
  | AH       | Authentication            |     -    |     51      |
  +----------+---------------------------+----------+-------------+
  ```

- IKE(Internet Key Exchange)
- ESP(Encapsulating Security Payload)
- AH(Authenticaton Header)
- SA(Security Association)
- ISAKMP(Internet Security Association and Key Management Protocol)

## L2TP/IPsec(Layer 2 Transport Protocol / IP Security)
- L2TP/IPSec PSK
- L2TP/IPSec RSA

## SSL VPN(Secure Socket Layer VPN)

## PPTP(Point to Point Transfer Protocol)

## IP-VPN / MPLS（Multi Protocol Label Switching
- いわゆる閉域網．

## OpenVPN, SoftEther, etc...


## software
- [All ProtonVPN apps are 100% open source - ProtonVPN Blog](https://protonvpn.com/blog/open-source/)
