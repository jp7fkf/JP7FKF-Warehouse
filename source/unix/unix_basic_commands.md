# Unix Basic Commands

## df -h
  - 容量みれる

## du -sh *
  - 今の階層下のフォルダやファイルの容量をみる．

## traceroute
  - `traceroute -a x.x.x.x` でAS名も一緒に引ける．(-A でas_serverを指定できる)
  - 尻尾になんかついてきたら
    ```
    - !H: [Type=3/Code=1] Host Unreachable 
    - !X: [Type=3/Code=13] Communication Administratively Prohibited
    - !N: [Type=3/Code=0] Net Unreachable
    - !P: [Type=3/Code=2] Protocol Unreachable
    - !F: [Type=3/Code=4] Fragmentation Needed and DF set
    - !S: [Type=3/Code=5] Source route failed
    - !V: [Type=3/Code=14] Host Precedence
    - !<icmp_code_num>
    ```

## tmux とかnohupするの忘れて長時間かかるコマンドをやってしまったとき
```
$ rpmbuild -ba hoge.spec // やっちまった
$ # Ctrl+Zで中断
$ bg 1
$ jobs 1
$ disown %1
```
- nohupとdisownの違いは
  - nohup:コマンドをハングアップシグナル無視で実行させる。
  - disown:nohupをつけ忘れた時、途中からバックグラウンドでの実行へ切り替える

## PIDファイルが残っている/subsys locked
```
$ service <service_name> status
<service_name> が停止していますが PID ファイルが残っています
$ rm /var/run/<service_name>.pid
rm: cannot remove `/var/run/<service_name>.pid': Permission denied
$ sudo rm /var/run/<service_name>.pid
$ service <service_name> status
<service_name> は停止していますがサブシステムがロックされています
$ sudo rm /var/lock/subsys/<service_name>
$ service <service_name> status
<service_name> は停止しています
$ sudo service <service_name> restart
```

## サブネット全部にpingしたい
  - `for a in `seq 1 254`; do ping -c 1 -w 1 10.0.255.$a > /dev/null && arp -a 10.0.255.$a | grep ether; done`
    - 便利なワンライナー．wrap/aliasしてもいいかも．

## curlでproxy経由する
  - `curl -x <proxy>:<port> -L http://example.com`

## iptables
```
$ vi /etc/sysconfig/iptables #ここにinitのtablesが書かれている
$ iptables-save > filename #export
$ iptables-restore < filename #import
$ service iptables save # 現在のiptablesの情報をinitにする(たぶん/etc/sysconfig/iptablesに書きに行く？)
  iptables: Saving firewall rules to /etc/sysconfig/iptables:[  OK  ]
```

## 新しいユーザ
```
useradd <user> 
  # -m: ホームディレクトリも一緒に作る
  # -s <path_to_shell> shell指定
  # -p: ハッシュ化したパスワード指定
# sudo できるように
usermod -G wheel <user> #wheelとかsudoとか必要があれば
vi /etc/sudors
  #100行目前後
  # %wheel        ALL=(ALL)       ALL
passwd <user> でパス変更しておこう
```
  - `usermod -aG <GROUPS> <USER> #GROUPSはカンマ区切りで複数指定可`
  - `gpasswd -a <USER> <GROUP>`
  - `adduser <USER> <GROUP>`

## デフォルトshellを変える
  - `chsh -s <path_to_shell> <user_name>`
  - もしくは `/etc/passwd`の該当ユーザの末尾に`<path_to_shell>`を追加
  - `cat /etc/shells`で使えるshell一覧

## sudors
```
基本
 $ 誰が どのホストで = (誰として) 何を
# rootユーザはどこでも、誰としてでも何でもできる
 $ root ALL=(ALL:ALL) ALL
# wheelグループのユーザはどこでも、誰としてでもパスワード無しで何でもできる
# %つけるとgroup指定．誰としての部分は(user:group)
 $ %wheel ALL=(ALL:ALL) NOPASSWD: ALL
# bobはaliceとして閲覧コマンドを実行できる
 $ bob ALL=(alice) /bin/ls, /bin/cat
```

## ping のTTL
  - TTLのデフォルト値（だいたい）
    - Linux：64
    - Windows：128
    - Solaris：255

## httping
  - httpでpingする感じ．
  - `httping <address>` で基本的には動く．
    - headメソッドでリクエストを投げる．
  - `-G` オプションでGETを投げる．
  - 他にもプロキシ指定(-x proxyserver:port)したり，タイムアウト(-t)，インターバル(-i)，http status code(-s)，URL指定(-g)などのオプションがある．

## find
  
```
# you can filter out messages to stderr. I prefer to redirect them to stdout like this.
$ find / -name art  2>&1 | grep -v "Permission denied"

# Explanation:
# In short, all regular output goes to standard output (stdout). All error messages to standard error (stderr).
# grep usually finds/prints the specified string, the -v inverts this, so it finds/prints every string that doesn't contain "Permission denied".
# All of your output from the find command, including error messages usually sent to stderr (file descriptor 2) go now to stdout(file descriptor 1) and then get filtered by the grep command.
# This assumes you are using the bash/sh shell.

# Under tcsh/csh you would use
$ find / -name art |& grep ....
 ```

## 容量の大きいディレクトリtop10
  - なんのせいでdiskが逼迫しているかわからないときよくある．
  - `du -m / | sort -nr | head -10`