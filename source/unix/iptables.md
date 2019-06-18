# iptables

## tutorial
- https://en.wikipedia.org/wiki/Netfilter#/media/File:Netfilter-packet-flow.svg
- iptablesを理解する上ではこの図がとても重要．
- iptables, ebtables
- http://ebtables.netfilter.org/br_fw_ia/br_fw_ia.html
- https://qiita.com/shotaTsuge/items/4d51e5dd24506a6fb07c
- https://www.turbolinux.co.jp/products/server/11s/user_guide/iptablescmd.html

## configuration 
- https://qiita.com/Tocyuki/items/6d90a1ec4dd8e991a1ce
- 現在のルール一覧を見る．
  - `iptables -L`
- ルールを追加する
  - `iptables -A <chain_name> -p <protocol> -j <target>`
- 行番号指定して追加する
  - `iptables -I INPUT <line_num> -s <ipaddress/netmask> -p <protocol> -m <protocol> --dport <port_num> -j <target>`
- 行番号指定して消す
  - `iptables -D <chain_name> <line_num>`

- 行番号もだす
  - `iptables -L --line-num`
```
[jp7fkf@host ~]$ iptables -h
iptables v1.4.21

Usage: iptables -[ACD] chain rule-specification [options]
       iptables -I chain [rulenum] rule-specification [options]
       iptables -R chain rulenum rule-specification [options]
       iptables -D chain rulenum [options]
       iptables -[LS] [chain [rulenum]] [options]
       iptables -[FZ] [chain] [options]
       iptables -[NX] chain
       iptables -E old-chain-name new-chain-name
       iptables -P chain target [options]
       iptables -h (print this help information)

Commands:
Either long or short options are allowed.
  --append  -A chain    Append to chain
  --check   -C chain    Check for the existence of a rule
  --delete  -D chain    Delete matching rule from chain
  --delete  -D chain rulenum
        Delete rule rulenum (1 = first) from chain
  --insert  -I chain [rulenum]
        Insert in chain as rulenum (default 1=first)
  --replace -R chain rulenum
        Replace rule rulenum (1 = first) in chain
  --list    -L [chain [rulenum]]
        List the rules in a chain or all chains
  --list-rules -S [chain [rulenum]]
        Print the rules in a chain or all chains
  --flush   -F [chain]    Delete all rules in  chain or all chains
  --zero    -Z [chain [rulenum]]
        Zero counters in chain or all chains
  --new     -N chain    Create a new user-defined chain
  --delete-chain
            -X [chain]    Delete a user-defined chain
  --policy  -P chain target
        Change policy on chain to target
  --rename-chain
            -E old-chain new-chain
        Change chain name, (moving any references)
Options:
    --ipv4  -4    Nothing (line is ignored by ip6tables-restore)
    --ipv6  -6    Error (line is ignored by iptables-restore)
[!] --protocol  -p proto  protocol: by number or name, eg. `tcp'
[!] --source  -s address[/mask][...]
        source specification
[!] --destination -d address[/mask][...]
        destination specification
[!] --in-interface -i input name[+]
        network interface name ([+] for wildcard)
 --jump -j target
        target for rule (may load target extension)
  --goto      -g chain
                              jump to chain with no return
  --match -m match
        extended match (may load extension)
  --numeric -n    numeric output of addresses and ports
[!] --out-interface -o output name[+]
        network interface name ([+] for wildcard)
  --table -t table  table to manipulate (default: `filter')
  --verbose -v    verbose mode
  --wait  -w [seconds]  wait for the xtables lock
  --line-numbers    print line numbers when listing
  --exact -x    expand numbers (display exact values)
[!] --fragment  -f    match second or further fragments only
  --modprobe=<command>    try to insert modules using this command
  --set-counters PKTS BYTES set the counter during insert/append
[!] --version -V    print package version.
```
- https://knowledge.sakura.ad.jp/4048/
```
# (1) ポリシーの設定 OUTPUTのみACCEPTにする
*filter
:INPUT   DROP   [0:0]
:FORWARD DROP   [0:0]
:OUTPUT  ACCEPT [0:0]

# (2) ループバック(自分自身からの通信)を許可する
-A INPUT -i lo -j ACCEPT

# (3) データを持たないパケットの接続を破棄する
-A INPUT -p tcp --tcp-flags ALL NONE -j DROP

# (4) SYNflood攻撃と思われる接続を破棄する
-A INPUT -p tcp ! --syn -m state --state NEW -j DROP

# (5) ステルススキャンと思われる接続を破棄する
-A INPUT -p tcp --tcp-flags ALL ALL -j DROP

# (6) icmp(ping)の設定
# hashlimitを使う
# -m hashlimit                   hashlimitモジュールを使用する
# —hashlimit-name t_icmp  記録するファイル名
# —hashlimit 1/m               リミット時には1分間に1パケットを上限とする
# —hashlimit-burst 10        規定時間内に10パケット受信すればリミットを有効にする
# —hashlimit-mode srcip    ソースIPを元にアクセスを制限する
# —hashlimit-htable-expire 120000   リミットの有効期間。単位はms
-A INPUT -p icmp --icmp-type echo-request -m hashlimit --hashlimit-name t_icmp --hashlimit 1/m --hashlimit-burst 10 --hashlimit-mode srcip --hashlimit-htable-expire 120000 -j ACCEPT

# (7) 確立済みの通信は、ポート番号に関係なく許可する
-A INPUT -p tcp -m state --state ESTABLISHED,RELATED -j ACCEPT

# (8) 任意へのDNSアクセスの戻りパケットを受け付ける
-A INPUT -p udp --sport 53 -j ACCEPT

# (9) SSHを許可する設定
# hashlimitを使う
# -m hashlimit                   hashlimitモジュールを使用する
# —hashlimit-name t_sshd 記録するファイル名
# —hashlimit 1/m              リミット時には1分間に1パケットを上限とする
# —hashlimit-burst 10       規定時間内に10パケット受信すればリミットを有効にする
# —hashlimit-mode srcip   ソースIPを元にアクセスを制限する
# —hashlimit-htable-expire 120000   リミットの有効期間。単位はms
-A INPUT -p tcp -m state --syn --state NEW --dport 22 -m hashlimit --hashlimit-name t_sshd --hashlimit 1/m --hashlimit-burst 10 --hashlimit-mode srcip --hashlimit-htable-expire 120000 -j ACCEPT

# (10) 個別に許可するプロトコルとポートをここに書き込む。
# この例では、HTTP(TCP 80)とHTTPS(TCP 443)を許可している。
-A INPUT -p tcp --dport 80   -j ACCEPT
-A INPUT -p tcp --dport 443  -j ACCEPT

COMMIT
```
- ip6tablesでも同じようにできるよ！ -pでicmpなのかicmpv6なのかとか，そのあたり注意．

### iptablesの永続化
- iptablesで指定したルールはOSを再起動すると消える．
  - iptablesの中身は下記に書き込まれる。
    - `/etc/iptables/rules.v4`
- Ubuntuではiptables-persistentパッケージがある
  - `sudo apt install iptables-persistent`
- これでsave,reloadできる．
```
sudo /etc/init.d/netfilter-persistent save
sudo /etc/init.d/netfilter-persistent reload
```