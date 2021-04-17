# SMTP

## basic
- [RFC 5321 - Simple Mail Transfer Protocol](https://tools.ietf.org/html/rfc5321)
  - リプライコードとかコマンドみるならここ．

## telnetでemailを送る
telnetでsmtpサーバに接続してsmtp formatに従ってメッセージを送ることができる．
```
% telnet <smtp_server> 25
Trying <smtp_server_ip>...
Connected to <smtp_server>.
Escape character is '^]'.
220 smtp ESMTP Postfix
EHLO jp7fkf.dev # (1)
250-smtp
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250-DSN
250-SMTPUTF8
250 CHUNKING
MAIL FROM:jp7fkf@jp7fkf.dev # (2)
250 2.1.0 Ok
RCPT TO:jp7fkf@gmail.com # (3)
250 2.1.5 Ok
DATA # (4)
354 End data with <CR><LF>.<CR><LF>
From: jp7fkf@jp7fkf.dev # (5)
To: jp7fkf@gmail.com
Subject: Test email from Telnet.

Test email from telnet.
. # (6)
250 2.0.0 Ok: queued as ADCF64A0BCC
quit
221 2.0.0 Bye
Connection closed by foreign host.
```

- (1): Helloを送る．具体的には `HELO` もしくは `EHLO` のあとに自ドメイン名を指定する．`EHLO`はSMTPの拡張を用いる際に指定する．
- (2): `MAIL FROM`として自アドレスを指定する．エンベロープFROMと呼ばれる．このコマンドの入力からメール送信のトランザクションが走る．
- (3): `RCPT TO`として宛先を指定する．エンベロープTOと呼ばれる．Recipient Toの略．
- (4): `DATA`からメールメッセージの記述に入る．
- (5): ヘッダとしてFrom, To, Cc, Bcc, Subject，その他のヘッダを指定できる．本文はヘッダのあとに改行を入れて指定する．ヘッダに書かれるFrom/ToはエンベロープFrom/Toと区別してヘッダーFrom/Toと呼ばれることもある．
- (6): 本文記述が終了したらピリオドを入力する．ピリオドがメッセージの終了を示すことになる．なお，行頭にてピリオド単体をメッセージ本文として利用したい場合は`..`とピリオドを2連続させるとメッセージ入力が終了せずにピリオド単体をメッセージとして使用することができる．行頭出ない場合は通常通りピリオドの利用が可能である．

### Plaintext Authの生成
認証つきSMTPサーバを利用する場合においては認証が必要である．
Plain Textによる認証を行う際に利用する認証文字列は下記により生成できる．
`printf "%s\0%s\0%s" <username> <username> <password> | openssl base64 -e | tr -d '\n'; echo`
plain textによる認証を行うためにはメール送信トランザクション開始前(MAIL FROM前)に `AUTH PLAIN: <auth_string>` を入力して指定する．

### References
- [telnetからSMTPを使ってメール送信 - Qiita](https://qiita.com/sheepland/items/973198fa80f0213fe5a1)
- [コマンドラインからSMTP認証の試験を行う - Qiita](https://qiita.com/azumakuniyuki/items/99c341e99a459d6214bc)
