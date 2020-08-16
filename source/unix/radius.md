# Radius

## free-radius install battle on CentOS7
  - [CentOS7 に FreeRadius をインストールする - らくがきちょう](http://sig9.hatenablog.com/entry/2016/10/20/120000)
  - `yum info freeradius`
  - `yum install freeradius`
  - `/etc/raddb/radiusd.conf`: raduis本体の設定
  - `/etc/raddb/clients.conf`: radiusクライアントの設定
  - `/etc/raddb/users`: ユーザの設定

## freeradius
- dictionaryのありか
  - `$ ls /usr/share/freeradius/`

## freeradius install buttle on ubuntu18.04
```
$ sudo apt install freeradius

# userのコメントアウトをはずす
$ cat /etc/freeradius/3.0/users
...(snip)...
#
# The canonical testing user which is in most of the
# examples.
#
bob Cleartext-Password := "hello"
  Reply-Message := "Hello, %{User-Name}"
#
...(snip)...

# radtestでテスト．
# ユーザは先程コメントアウトを外したbob, secretはclients.confデフォルトのtesting123
$ radtest bob helle 127.0.0.1 1812 testing123
Sent Access-Request Id 89 from 0.0.0.0:54129 to 127.0.0.1:1812 length 73
  User-Name = "bob"
  User-Password = "helle"
  NAS-IP-Address = 127.0.1.1
  NAS-Port = 1812
  Message-Authenticator = 0x00
  Cleartext-Password = "helle"
Received Access-Reject Id 89 from 127.0.0.1:1812 to 0.0.0.0:0 length 32
  Reply-Message = "Hello, bob"
(0) -: Expected Access-Accept got Access-Reject

$ radtest bob hello 127.0.0.1 1812 testing123
Sent Access-Request Id 22 from 0.0.0.0:45159 to 127.0.0.1:1812 length 73
  User-Name = "bob"
  User-Password = "hello"
  NAS-IP-Address = 127.0.1.1
  NAS-Port = 1812
  Message-Authenticator = 0x00
  Cleartext-Password = "hello"
Received Access-Accept Id 22 from 127.0.0.1:1812 to 0.0.0.0:0 length 32
  Reply-Message = "Hello, bob"

```

## cisco デバイスでradius(freeradius)認証を使うコツ
```
$ echo -n helloworld | openssl md5
(stdin)= fc5e038d38a57032085441e7fe7010b0
$ echo -n imalice | openssl md5
(stdin)= 4951f932f296e0f05450897c9a0462e5

$ cat /etc/freeradius/3.0/users
...(snip)...
bob Cleartext-Password := "imbob"
  Service-Type = NAS-Prompt-User,
  Reply-Message = "Hello, %{User-Name}"

alice MD5-Password := "4951f932f296e0f05450897c9a0462e5" #imalice
  Service-Type = NAS-Prompt-User,
  Reply-Message = "Hello, %{User-Name}",
  cisco-avpair = "shell:priv-lvl=15"

$enab15$  MD5-Password := "fc5e038d38a57032085441e7fe7010b0" # helloworld
    Service-Type = NAS-Prompt-User,
    Reply-Message = "Hello, %{User-Name}"
...(snip)...

$ ssh {host_ip} -l bob
Password:
Hello, bob
cisco-sw>ena
Password: Hello, $enab15$

cisco-sw#exit

$ ssh {host_ip} -l alice
Password:
Hello, alice
cisco-sw#
```
- ref: [Cisco IOS and Radius](https://wiki.freeradius.org/vendor/Cisco)
- ref: [AAA Interface Administration and Reference, StarOS Release 21.3  - RADIUS Dictionaries and Attribute Definitions [Cisco ASR 5000 Series] - Cisco](https://www.cisco.com/c/en/us/td/docs/wireless/asr_5000/21-3_N5-5/AAA/21-3-AAA-Reference/21-AAA-Reference_chapter_01110.html#reference_eadf5559-d5f8-40d6-a3d0-c9d117f108de)
  - radius dictionary example of cisco devide