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
  - sha2 とかrsaとしてのハッシュ値が保存されていて，ホストの信頼性を保証しようとしている
  - そもそもホスト鍵って何者？ホストの公開鍵を引っ張ってくるならわかる．
  - でもパスワード認証だけの場合はここに何が入るの？ -> ssh_keygenとか全くやってないから公開鍵なんてどこにもないんじゃないの？

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
- [多段sshを行うときに、ローカルの秘密鍵を参照し続ける - Qiita](https://qiita.com/ymd_/items/5eb833ad757bd8b3e6c3)
- [ssh-agentの使い方 - Qiita](https://qiita.com/isaoshimizu/items/84ac5a0b1d42b9d355cf)
- sshに`-A`オプションをつけるのが一番手っ取り早い．
- `ForwardAgent yes` を`/etc/ssh/sshd_config` or `~/.ssh/config` とかに書く．

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

## sshrc
- `brew install sshrc`
- `~/.sshrc` に記述したコマンドをssh直後に自動で実行する．
```
$ mkdir ~/.sshrc.d
$ cd ~/.sshrc.d && ln -s ../.vimrc .vimrc/
$ cat << 'EOF' >> ~/.sshrci
export VIMINIT="let \$MYVIMRC='$SSHHOME/.sshrc.d/.vimrc' | source \$MYVIMRC"
EOF
$ cat ~/.sshrc
export VIMINIT="let \$MYVIMRC='$SSHHOME/.sshrc.d/.vimrc' | source \$MYVIMRC"
```

## ssh 先にvimrcとかbashrcをもっていく
- [sshした先に.bashrcや.vimrcを持って行きたい人のためのsshrc - Qiita](https://qiita.com/ikuwow/items/ba4ca57fd67c06fd1b19)

## ssh broken pipe
- [How to prevent "Write Failed: broken pipe" on SSH connection? - Ask Ubuntu](https://askubuntu.com/questions/127369/how-to-prevent-write-failed-broken-pipe-on-ssh-connection)
```
Earlier I was able to ssh to my droplet using ssh root@xxx.xx.xx.xxx command. But from yesterday I am getting this error packet_write_wait: Connection to xxx.xx.xx.xxx port 22: Broken pipe. What could be the possible reason for it. And how to solve it?
kamaln7 MOD February 14, 2018
Hi, if you're getting that error it means that your SSH connection was cut off due to a long period of inactivity. You can prevent that from happening by configuring either the SSH server or client to check if the other is still connected periodically.
To do so on the server, add the following two lines to /etc/ssh/sshd_config:
```
```
ClientAliveInterval 300
ClientAliveCountMax 2
```
On the client side, if you are using the ssh command line program, add the following lines to `~/.ssh/config`. This will enable this feature for all remote hosts.
```
Host *
    ServerAliveInterval 300
    ServerAliveCountMax 2
```
Otherwise, if you're using a GUI program, it should have a setting for Keep Alive.
This will send a ping every 300 seconds (5 minutes) and disconnect after 2 failures (in case the other side actually disconnected and isn't simply inactive).

## sshしてログイン時にプロンプト帰ってくるのが遅い．
- `/etc/ssh/sshd_config` で `UseDNS no`する．これで逆引きしに行かなくなる．

## known_hostsにのってるfingerprintと違うと言われるとき
```
ubuntu@lab1:~$ ssh admin@10.10.10.1
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:+7mcUfxxx2BTLzW/9ky0locxxxI2xwyWNJuAacNJxxx.
Please contact your system administrator.
Add correct host key in /home/ubuntu/.ssh/known_hosts to get rid of this message.
Offending RSA key in /home/ubuntu/.ssh/known_hosts:3
  remove with:
  ssh-keygen -f "/home/ubuntu/.ssh/known_hosts" -R 10.10.10.1
RSA host key for 10.10.10.1 has changed and you have requested strict checking.
Host key verification failed.

ubuntu@lab1:~$ ssh-keygen -f "/home/ubuntu/.ssh/known_hosts" -R 10.10.10.1
# Host 10.10.10.1 found: line 3
/home/ubuntu/.ssh/known_hosts updated.
Original contents retained as /home/ubuntu/.ssh/known_hosts.old

ubuntu@lab1:~$ ssh admin@10.10.10.1
```

## sshの known_hostsから消す
- `ssh-keygen -f "/home/jp7fkf/.ssh/known_hosts" -R "10.255.255.1`
