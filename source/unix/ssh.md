# ssh
## 鍵ペアをつくる
- RSA でビット長4096なやつ
```
ssh-keygen -t rsa -b 4096 {-C "email@example.com"} //コメントつけたければつける
```


## どんな鍵だったっけ
```
ssh-keygen -l -v -f ~/.ssh/id_rsa.pub
```
  - -v つけるとフィンガープリントが出る．

## known_hosts って
  - 何が書かれてるんだ？  
  sha2 とかrsaとしてのハッシュ値が保存されていて，ホストの信頼性を保証しようとしている
  そもそもホスト鍵って何者？ホストの公開鍵を引っ張ってくるならわかる．
  でもパスワード認証だけの場合はここに何が入るの？ -> ssh_keygenとか全くやってないから公開鍵なんてどこにもないんじゃないの？

## sshuttle
  - べんり
  - `sshuttle -r <username>@<host> <forwarding_ip_range>`

## port forward
```
$ ssh -L <localport>:<forwarded_host>:<forwarded_port> <username>@<dst_host>
```

## sshconfig
```
## port forwarding
Host NAME
    HostName Address1
    User User1
    LocalForward PortA Address2:PortB 

## proxy する
Host server
   HostName server.co.jp
   User hoge
Host proxy_target
   HostName target.co.jp
   User hoge
   ProxyCommand ssh -W %h:%p server
   ```

## agent transfer
 - https://qiita.com/ymd_/items/5eb833ad757bd8b3e6c3
 - https://qiita.com/isaoshimizu/items/84ac5a0b1d42b9d355cf

## ワイルドカードを使う
```
 host gateway
   HostName sshgate.hoge
   User hoge

Host RemoteHost*
  HostName RemoteHost
  User fuga

host *-none
   ProxyCommand none

Host *-out*
   ProxyCommand ssh -W %h:%p gateway
```

## permission
  - `~/.ssh` は`700`
  - `~/.ssh/authorized_keys` は`644`