# Wireless
- Wireless LAN, 802.11な話

## 802.11b

## 802.11a

## 802.11n

## 802.11ac

## wifiの規格とアライアンス

## wifi6の話

## Roming/Handover

## 2019-09-03 14:06:21 YudaiHashimoto
### ワイヤレス基礎勉強会
Cisco: Enterprise Networking Wireless 2019/03/29．

10%くらいカバレッジをかぶらせるとroaming，ハンドオーバがスムーズになる．
高速ローミング802.11r, 11k, 11vらへん．

sticky client問題

ciscoでできる端末振り分け機能
- バンドセレクト
- クライアントロードバンランス
- 自動出力，CH設定(RPM: Radio Resource Management)
- 最大クライアントの設定
- 程データレートの無効化．
- RSSI Low Check
- Rx SOP
- Optimized Roaming
- 802.11r/k/v

- Wi-Fi6: 802.11ax
  - channel sliceing

Q. セルの大きさを決めるのは，RSSI強度でよい？
A. ANTとかRSSIとかleast datarateとか．結局総合的に．

Q. Channel Slicingとは．MIMO的なものとは違う？
A. OFDMAだった．チャネルあたり9クライアント同時片方向通信ができる．

Q. じゃあ，MIMOとOFDMAを組み合わせてやっている？
A. 
- 2.4G, 5Gとかチップ別，そのほかにスペアナ用とか，コントロール用のCPUとかが載っており，放熱が厳しくて重たかった．
  - 新しいやつはkaizenされている． custom rf asic これってcisco独自？
  - 2800, 3800は色々機能がついている．