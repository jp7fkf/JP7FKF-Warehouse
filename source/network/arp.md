# arp

## Unicast ARP
- 特にtableのaging timerがexpireするときに更新をするときなんかはunicast ARPになることが多いらしい（実装依存）
- あとはARP Replyのときとか．

## tips
- ARP の送信感覚は20secとかそこいら．
- 一回ARP Req. 投げて応答がなかったら，20secくらい待つハメになるなどする．

## arp cache を消したい！
### Linux(CentOS7)
- `ip -s -s neigh flush all`

### macos
- `sudo arp -d -a`

### インタフェース指定して消す
- `sudo arp -d -i en0 -a`
