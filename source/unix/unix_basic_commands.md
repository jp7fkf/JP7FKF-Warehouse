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
- `-I`: オプションで強制的にICMPで投げる．
- TODO: traceroute のフィールドがわからない．なんでICMP強制したら`*`になったんだ．パケットを見させてくれたのむ

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

## grep
- `grep -r <search_str> <search_dir_1> <search_dir_2>...`

## find
- `find <search_dir> -name <file_name>`
  
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

## シンボリックリンク
- `ln -s <origin> <link_name(optional)>`
- `unlink <link_name>`

## less
  - `-r` オプションをつけるとカラーとかが制御文字で表示されなくていい．

## ユーザ系
### 新しいユーザ
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

### デフォルトshellを変える
- `chsh -s <path_to_shell> <user_name>`
- もしくは `/etc/passwd`の該当ユーザの末尾に`<path_to_shell>`を追加
- `cat /etc/shells`で使えるshell一覧

### sudors
- `visudo` する
```
 $ 誰が どのホストで = (誰として) 何を
# rootユーザはどこでも、誰としてでも何でもできる
 $ root ALL=(ALL:ALL) ALL
# wheelグループのユーザはどこでも、誰としてでもパスワード無しで何でもできる
# %つけるとgroup指定．誰としての部分は(user:group)
 $ %wheel ALL=(ALL:ALL) NOPASSWD: ALL
# bobはaliceとして閲覧コマンドを実行できる
 $ bob ALL=(alice) /bin/ls, /bin/cat
```

### groups
- ユーザのグループ確認
  - `groups <user_name>`
  - もちろん `/etc/group` でも見れる
- ユーザをグループに追加
  - `usermod -aG <group_name> _<user_name>`
   - groupsは複数指定できる(comma separation)
  - `gpasswd -a <user_name> <group_name>`
  - どっちでもいっしょ．`usermod`使うときは `-a`を忘れないようにしないとセカンドグループが上書きになるので注意．`-a`はaddの意．
- グループに所属しているuserを確認
  - `getent group <group_name>`
- グループに入る(login)
  - `newgrp <group name>`
    - グループにログインした状態でファイル等を作成すると，そのファイルのプライマリグループがログインしたグループとなる．
- グループのパスワードを設定
  - `gpasswd <group_name>`

## git
### 脳死commit
- いいからかけ．脳死コミットができあがる．
  - ``alias gitcmtnow='git commit -m "`date "+%Y-%m-%d %H:%M:%S"`"'``
  - 脳死してcommitしたいときに便利．zshrcやらbashrcに書いておこう．脳死しよう．

### git ignoreをglobalにかく
1. `~/.gitconig` にexcludersfileを書く．
2. 上記で書いたexcludersfileにgitignoreを書く．

- `$ git config --global core.excludesfile ~/.gitignore_global`

``` .gitconfig
[core]
    excludesfile = /Users/<username>/.gitignore_global
```

- .gitignore_global に .DS_Store を追加する
``` .gitignore_global
.DS_Store
```

## diff
- フォルダごとdiffとりたい
  - `diff -r <dir_1> <dir_2>`

## netstat
- `netstat -rn`
- `netstat -tuna`

- Options
  - `-n, --numeric`: show with numeric address. no dns lookup (more speedy!)
  - `-t, --tcp`: tcp
  - `-u, --udp`: udp
  - `-a, --all`: all information
  - `-i, --interface <interface_name>`: designate interface
  - `-s, --statistics`: show statictics
  - `-c, --continuous`: show with continuouts refreshments.
  - `-o, --timers`: show network timers
  - `-e, --extend`: show extended information
  - `-l, --listening`: show listening sockets
  - 

## wc
- word count
- Options
  - `-l`: counts num of lines
  - `-c`: counts bytes
  - `-m`: counts num of characters
  - `-w`: counts num of words

## watch
### Options
  - `-d, --differences[=permanent]`: 変更があった場合にハイライトする. `=permanent` つけておくと当たり前だがずっとハイライトが残る．
  - `-n, --interval <seconds>`
  - `-t, --no-title`
  - `-b, --beep`: Beep if command has a non-zero exit.
  - `-e, --errexit`: Freeze updates on command error, and exit after a key press.
  - `-g, --chgexit`: Exit when the output of command changes.
  - `-c, --color`: Interpret ANSI color and style sequences.
  - `-x, --exec`: Pass command to exec(2) instead of sh -c which reduces the  need to use extra quoting to get the desired effect.

### EXIT STATUS
```
0      Success.
1      Various failures.
2      Forking the process to watch failed.
3      Replacing  child  process  stdout  with  write  side pipe
       failed.
4      Command execution failed.
5      Closing child process write pipe failed.
7      IPC pipe creation failed.
8      Getting  child  process  return  value  with   waitpid(2)
       failed, or command exited up on error.
other  The  watch  will  propagate  command exit status as child
       exit status.
```