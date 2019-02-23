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

## 代表的なdns ソフトウェア
- Bind
  - 有名．脆弱性がちょっと多い印象．
- Power DNS
  - Web GUIがあり，レコード登録などの取り扱いがやりやすい．
- Unbound
  - DNSキャッシュサーバ．コンテンツサーバにはなれない．
  - DNSキャッシュサーバとしては，BINDを完全に置き換えることができる程度の機能は実装されている．
- Knot DNS
  - 権威DNS（コンテンツサーバ）専用
  - キャッシュサーバ(resolver)として使いたい場合は`Knot DNS resolver` というものもある．
    - cloudflare(1.1.1.1)で使われているらしい．

## Primary DNSはどっち？
  - `dig @<server> example.com ns`
    ```
      #...
      #;; ANSWER SECTION:
      #example.com.   87468 IN  NS  ns2.example.com.
      #example.com.   87468 IN  NS  ns1.example.com.
      #...
    ```
  - `dig @<server> ns1.example.com soa`
    ```
      # example.com.   180 IN  SOA ns1.example.com. hoge.example.com. ...
    ```
  - `dig @<server> ns2.example.com soa`
    ```
      # example.com.   180 IN  SOA ns1.example.com. hoge.example.com. ...
    ```

  - この場合，SOAがns1なので，ns1がprimary と機能していると見ることができる．
  
## ゾーン転送
  - `dig @<server> <domain> axfr`

## bind9 build battle
  - bind9.13らへんをいれたかった．
  - http://linux-sxs.org/internet_serving/bind9.html
  ```
  $ wget https://www.openssl.org/source/openssl-1.0.2q.tar.gz
  $ tar xvzf openssl-1.0.2q.tar.gz
  $ cd openssl-1.0.2q/
  $ perl util/perlpath.pl `which perl` //はいらなかった
  $ ./config --prefix=/usr --openssldir=/usr/ssl shared
  $ make
  $ su
  $ rpm -q -a | grep openssl | while read line; do rpm -e --nodeps $line; done
  $ make install
  $ ldconfig -v

  $ wget ftp://ftp.isc.org/isc/bind9/9.13.2/bind-9.13.2.tar.gz
  $ tar xvzf bind-9.13.2.tar.gz
  $ cd bind-9.13.2/
  $ yum provides *sys/capability.h
  $ yum install libcap-devel
  $ ./configure --prefix=/usr --sysconfdir=/etc --enable-threads --localstatedir=/var/state --with-libtool --with-openssl=/usr/
  $ make
  $ su
  $ rpm -q -a | grep '^bind' | while read line; do rpm -e --nodeps $line; done
  $ ldconfig -v
  $ groupadd named
  $ vigr
  $ chown root:daemon /var/run
  $ chmod 775 /var/run
  $ mkdir -p /var/named/pz
  $ chown -R named:named /var/named
  $ chmod -R 755 /var/named
  ```
