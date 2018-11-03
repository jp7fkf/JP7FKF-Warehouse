# DNS
## DNSに関するRFC
RFC1034, RFC1035, RFC1912, RFC2181あたりをざっくりと
RFC3484にはこう書かれている．
```
 Rule 8:  Use longest matching prefix.
   If CommonPrefixLen(SA, D) > CommonPrefixLen(SB, D), then prefer SA.
   Similarly, if CommonPrefixLen(SB, D) > CommonPrefixLen(SA, D), then
   prefer SB.
```
つまるところ，複数のIPアドレスリストをDNS answerとしてクライアントに返す場合，これまでは'answerのIPアドレスリストのうち最初のIPアドレスから順に使うクライアントが"多かった"'のだが，RFC3484に準拠した実装が行われているクライアントでは，自分のIPアドレスとのロンゲストマッチをとったIPアドレスリストのうちの最初のものから順に使って行くようである．DNSラウンドロビンなどで複数のIPアドレスリストを返す場合に，DNSサーバが構成したリストの最初の1番目が利用されるわけではなくなるということになる．
これを知っておかないと意図した動作と異なるふるまいをしそうなので注意が必要そう．
- [CNAME を巡る 2/3 のジレンマ - 鷲ノ巣](https://tech.blog.aerie.jp/entry/2014/09/09/162135)

- [RFC 1912 -  Common DNS Operational and Configuration Errors](https://www.ietf.org/rfc/rfc1912.txt)