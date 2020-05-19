# Elastic Stack
- Elasticsearch, Logstash, Kibana, Beatsなどのあれこれ．

## Elasticsearch
### dump
- レポジトリを作ってデータバックアップ，リストア
  - [Elasticsearchのデータをバックアップして、別ノードにリストアする - togatttiのエンジニアメモ](http://togattti.hateblo.jp/entry/2016/11/09/120710)
  - [Elasticsearch スナップショット 5 分で取得する | cloudpack.media](https://cloudpack.media/7139)
- elasticdumpを使う方法
  - [よく使うElasticSearchのクエリ（elasticdump） - Qiita](https://qiita.com/nakazii-co-jp/items/3199433d685d0600c6d6)
  - jsonとかでとれる．
大容量ファイルのときにどっちの手法がいいのかわからない．多分レポを作ってやるほうかな．

## Logstash
### Plugins
#### Installation using logstash-plugin command
- Example: logstash-output-slack
```
root@elk:/usr/share/logstash/bin# ./logstash-plugin install logstash-output-slack
Validating logstash-output-slack
Installing logstash-output-slack
```
####  cisco log
- [Logstash: Processing Cisco Logs](https://gist.github.com/justinjahn/85305bc7b7df9a6412baedce5f1a0ece)

#### geoip
- https://www.elastic.co/jp/blog/geoip-in-the-elastic-stack
- [Geoip filter plugin | Logstash Reference [6.2] | Elastic](https://www.elastic.co/guide/en/logstash/6.2/plugins-filters-geoip.html)
- geolite.maxmind.com
- [Kibana + Elasticsearch + Logstash を使って Netflow を可視化する - kodamapのブログ](http://kodamap.hatenablog.com/entry/2017/07/11/230705)
- [GeoLite2 Free Downloadable Databases « MaxMind Developer Site](https://dev.maxmind.com/geoip/geoip2/geolite2/)

#### dns
- [Dns filter plugin | Logstash Reference [7.5] | Elastic](https://www.elastic.co/guide/en/logstash/current/plugins-filters-dns.html)

#### flow
- http://enog.jp/wp-content/uploads/2015/09/enog43_elk_0904.pdf
  - ASN, gio-ipはいれたい．
- https://www.janog.gr.jp/meeting/janog39/application/files/7014/8481/0318/janog39-traffic-nishizuka-03.pdf

#### slack通知
  - [logstash-plugins/logstash-output-slack](https://github.com/logstash-plugins/logstash-output-slack)
  - [Real examples needed Âˇ Issue #24 Âˇ cyli/logstash-output-slack Âˇ GitHub](https://github.com/cyli/logstash-output-slack/issues/24)

- こんな分岐のしかたもある．
```
if [dstip] and [dstip] !~ "(^127.0.0.1)|(^10.)|(^172.1[6-9].)|(^172.2[0-9].)|(^172.3[0-1].)|(^192.168.)|(^169.254.)" {
```

### Tools
- grok debugger
  - [Grok Debugger](https://grokdebug.herokuapp.com/)
- elastic search tips
  - kuromoji tokenizer: [ElasticsearchでKuromoji Tokenizerを試す - abcdefg.....](http://pppurple.hatenablog.com/entry/2017/05/28/141143)

### Others
- Multiple Pipeline
  - [絶対的に使った方がいいLogstashのMultiple Pipelinesについて書いてみた - Qiita](https://qiita.com/micci184/items/24e197a168891f089b3d)

- 振り分け系
  - [Logstashで内容ごとに送信先を複数に振り分ける設定 - designetwork](https://designetwork.daichi703n.com/entry/2017/04/10/logstash-multiple-output)

- flow on ELK
  - http://enog.jp/wp-content/uploads/2015/09/enog43_elk_0904.pdf
  - [sflow | Logstash Reference [5.2] | Elastic](https://www.elastic.co/guide/en/logstash/5.2/plugins-codecs-sflow.html)

- netflow module
  - [Logstash Netflow Module | Logstash Reference [7.5] | Elastic](https://www.elastic.co/guide/en/logstash/current/netflow-module.html)

- logstash 入門
  - [Logstash入門 - Qiita](https://qiita.com/nihei9/items/1ebc7934eb1e1ce66162)

### Refs
- [Elasticsearch Logstash Kibanaの環境構築 | Think IT（シンクイット）](https://thinkit.co.jp/article/13444)
- [elastiflow/INSTALL.md at master · robcowart/elastiflow](https://github.com/robcowart/elastiflow/blob/master/INSTALL.md)

## Common
### install battle on ubuntu18.04
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.0.deb
sha1sum elasticsearch-5.4.0.deb
sudo dpkg -i elasticsearch-5.4.0.deb
sudo vim /etc/elasticsearch/jvm.options
java -version
sudo apt install default-jre
sudo apt list --installed | grep jre
curl -XGET 'localhost:9200/?pretty'
sudo vim /etc/elasticsearch/elasticsearch.yml
sudo service elasticsearch start
```
- [Ubuntu18へのElasticStackインストール - Qiita](https://qiita.com/hogehogehugahuga/items/e4ce03269309e90d04b8)

### Tips
- [Elasticsearchで、正規表現での検索結果が思い通りにならなかったことへの対応 - Qiita](https://qiita.com/d2cd-ytakada/items/9042122b36ab10c2c580)

## beats
- [ELK構成に Beats(Filebeat)を導入しログ可視化させる - 備忘録／にわかエンジニアが好きなように書く](https://www.n-novice.com/entry/2018/03/01/005142)
- [最初に知っておけば良かったFilebeatの設定 - Taste of Tech Topics](http://acro-engineer.hatenablog.com/entry/2018/12/13/120000)

## Kibana
- [Grok の構文を検証する Grok Debugger を試してみた #Kibana ｜ DevelopersIO](https://dev.classmethod.jp/server-side/elasticsearch/x-pack-grok-debugger/)
- [elasticsearch – Kibana3を使ったアラート/通知 - コードログ](https://codeday.me/jp/qa/20190401/511451.html)

## elk memo misc
- installation: [DebianパッケージでのKibanaのインストール | Kibanaユーザーガイド [5.4] | Elastic](https://www.elastic.co/guide/jp/kibana/current/deb.html)
- installation to ubuntu18.04: [Ubuntu18へのElasticStackインストール - Qiita](https://qiita.com/hogehogehugahuga/items/e4ce03269309e90d04b8)
- [Elasticsearch Logstash Kibanaの環境構築 | Think IT（シンクイット）](https://thinkit.co.jp/article/13444)
- [インストール：Elasticsearch, Logstash, and Kibana (ELK Stack) ＋ Nginx(ReverseProxy) on CentOS7.4 - 備忘録／にわかエンジニアが好きなように書く](https://www.n-novice.com/entry/2018/02/22/214421#logstashインストール)
- beats: [Beatsを使ってみた（まとめ編） ｜ Developers.IO](https://dev.classmethod.jp/server-side/elasticsearch/beats-entry-matome/)
- metricbeat: [Metricbeatによるシステム情報の視覚化 - Qiita](https://qiita.com/hana_shin/items/95cf8e165333f2a9d1c1)
- kibanaの正規表現がいけてない．普通のregexpでは反応しない．
- visualize of netflow: [Kibana + Elasticsearch + Logstash を使って Netflow を可視化する - kodamapのブログ](http://kodamap.hatenablog.com/entry/2017/07/11/230705)
- log aggregator with logstash: [Logstashによるログ収集システムの構築 | 情シスハック](http://success.tracpath.com/blog/2014/06/04/logstash%E3%81%AB%E3%82%88%E3%82%8B%E3%83%AD%E3%82%B0%E5%8F%8E%E9%9B%86%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0%E3%81%AE%E6%A7%8B%E7%AF%89/)
- [15分で作る、Logstash＋Elasticsearchによるログ収集・解析環境 | さくらのナレッジ](https://knowledge.sakura.ad.jp/2736/)
- [Remote syslog to Logstash – Halon](https://support.halon.io/hc/en-us/articles/360000700065-Remote-syslog-to-Logstash)
- [Logstash+Elasticsearch+Kibanaでログを可視化してみましょう - Qiita](https://qiita.com/quotto/items/8250c67ced43dc83b770)
- https://protocol.nekono.tokyo/2018/11/01/logstashで複数ポートでsyslog転送を受け付ける/
- // 結局groksを上手に書いて多数のログ形式をサポートするのがつらい．

- installation logstash: [Logstashのインストールと簡単な設定方法 - Symfoware](https://symfoware.blog.fc2.com/blog-entry-1910.html)
- the first logstash: [はじめての Logstash - Qiita](https://qiita.com/nskydiving/items/0cb598de7ffb5c22424d)

- syslog: serverity, rsyslog, logger command
- geoip with logstash: [How To Map User Location with GeoIP and ELK (Elasticsearch, Logstash, and Kibana) | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-map-user-location-with-geoip-and-elk-elasticsearch-logstash-and-kibana)
- logstash grok: [LogstashでのGrokの始め方 - Qiita](https://qiita.com/tuneyukkie/items/75cbb4d44f901fec2188)
- [DHCP lease logからmac addr OUI/Vender を抽出する](https://www.securitydistractions.com/2019/01/05/adding-windows-dhcp-logs-to-elastic-part-2/)
  - OUIはこのあたりのリストが便利: [nmap/nmap-mac-prefixes at master · nmap/nmap](https://github.com/nmap/nmap/blob/master/nmap-mac-prefixes)

## ELK Install Battle
```
sudo apt install openjdk-8-jdk apt-transport-https
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
sudo apt-get update && sudo apt-get install elasticsearch
sudo vi /etc/elasticsearch/elasticsearch.yml
sudo service elasticsearch status
sudo service elasticsearch start
sudo service elasticsearch status
sudo service elasticsearch enable
curl "http://localhost:9200/"
sudo apt install kibana
sudo vi /etc/kibana/kibana.yml
sudo service kibana status
sudo service kibana start
sudo service kibana enable
curl http://localhost:5601/ -L
sudo apt install logstash
sudo vim /etc/systemd/system/logstash.service
sudo systemctl status logstash
sudo systemctl start logstash
sudo vim /etc/logstash/conf.d/syslog.conf
sudo vi /etc/elasticsearch/elasticsearch.yml
sudo systemctl start elasticsearch.service
sudo systemctl daemon-reload
sudo systemctl enable kibana
sudo systemctl start kibana
sudo systemctl start logstash
curl http://localhost:9200/applogs/_search?q=*
curl http://localhost:9200//_search?q=*
curl http://localhost:9200/syslog/_search?q=*
curl localhost:9200
```

## kea-dhcpのallocログからouiを取得してベンダをフィールドに追加する
- 前提
```
"message" => "INFO  [kea-dhcp4.leases] DHCP4_LEASE_ALLOC [hwtype=1 01:23:45:67:89:ab], cid=[01:01:23:45:67:89:ab], tid=0x11111111: lease 1.2.3.4 has been allocated"
```
  - keaのリースログとしてこれが流れてくる．

- sample.conf
```
input {
  elasticsearch {
    hosts => "localhost"
    query => '{ <Queries> }'
  }
}

filter {
  grok {
    "match" => { "message" => '.*\[hwtype.* %{COMMONMAC:macaddr}]' } # COMMONMACはdefaultのgrok-patternsとして存在する
  }

  mutate {
    add_field => ["macaddr_prefix", "%{macaddr}"]
  }

  mutate {
    gsub=> ["macaddr_prefix", "^(.{8}).*", "\1" ]
  }
}

  translate {
    dictionary_path => "/etc/logstash/oui.yml" # `"xx:xx:xx": "vender_name"` がリストされたymlへのpath
    field      => "macaddr_prefix"
    destination => "oui"
    fallback => "N/A"
  }
}

output{
  stdout { codec => rubydebug }
}
```
  - elasticsearchの既存indexからリースログを引くクエリを指定してinputとしている．
  - kibana等で可視化したい場合はoutputを別indexとしてelasticsearchに投げることでkibanaでも扱えるようになる．
    - この手法をとるとouiフィールドがあるindexと無いindexでほぼ同様の情報が2つdupで保存されてしまうことに注意する．

## elk install battle
- [インストール：Elasticsearch, Logstash, and Kibana (ELK Stack) ＋ Nginx(ReverseProxy) on CentOS7.4 - 備忘録／にわかエンジニアが好きなように書く](https://www.n-novice.com/entry/2018/02/22/214421#logstash%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)

## logstash x grok
- [LogstashでのGrokの始め方 - Qiita](https://qiita.com/tuneyukkie/items/75cbb4d44f901fec2188)


# 概要
- Ubuntu18.04に
- Elasticsearch(7.7)まわりのdebパッケージを
- aptレポジトリから引っ張ってきて
- インストールして
- Logstash(log insert) -> Elasticsearch(processiog) <-> Kibana(visualize)ができる
ところまでを実施します．


基本的に下記を参照します

- [Install Elasticsearch with Debian Package | Elasticsearch Reference [7.7] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html)


## Elastic Stack Install Battle
### Install PGP key
```
$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
OK
```
### add repository to apt source list
```
$ echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
deb https://artifacts.elastic.co/packages/7.x/apt stable main
```

### Install Elasticsearch
```
$ sudo apt update
$ sudo apt install elasticsearch
$ sudo apt install elasticsearch
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
  elasticsearch
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 314 MB of archives.
After this operation, 527 MB of additional disk space will be used.
Get:1 https://artifacts.elastic.co/packages/7.x/apt stable/main amd64 elasticsearch amd64 7.7.0 [314 MB]
Fetched 314 MB in 5min 57s (881 kB/s)
Selecting previously unselected package elasticsearch.
(Reading database ... 116882 files and directories currently installed.)
Preparing to unpack .../elasticsearch_7.7.0_amd64.deb ...
Creating elasticsearch group... OK
Creating elasticsearch user... OK
Unpacking elasticsearch (7.7.0) ...
Setting up elasticsearch (7.7.0) ...
Created elasticsearch keystore in /etc/elasticsearch/elasticsearch.keystore
Processing triggers for ureadahead (0.100.0-21) ...
Processing triggers for systemd (237-3ubuntu10.40) ...
```
ここまででelasticsearch自体のインストール自体は完了．

- ヒープサイズのconfig
JVMで使わせるヒープサイズをチューンする．一般には物理メモリの約半分を割り当てればいいそう．
ただし32GBらへんに圧縮オブジェクトポインタのしきい値があるらしく，だいたい26GB以下にしておけば安全そう．
- ref: [Setting the heap size | Elasticsearch Reference [7.7] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/heap-size.html)
今回は物理メモリとして16GB割り当てているので8GBにしておく．
```
$ sudo cat /etc/elasticsearch/jvm.options | head -30
## JVM configuration

################################################################
## IMPORTANT: JVM heap size
################################################################
##
## You should always set the min and max JVM heap
## size to the same value. For example, to set
## the heap to 4 GB, set:
##
## -Xms4g
## -Xmx4g
##
## See https://www.elastic.co/guide/en/elasticsearch/reference/current/heap-size.html
## for more information
##
################################################################

# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms8g
-Xmx8g

################################################################
## Expert settings
################################################################
##
## All settings below this section are considered
## expert settings. Don't tamper with them unless
```
- ref: [Setting JVM options | Elasticsearch Reference [master] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/master/jvm-options.html)

- `/etc/elasticsearch/elasticsearch.yml` を編集
デフォルトだとlocalhostからしかアクセスできないので，少なくとも外部から繋ぐ場合はbind ip addressを編集する．
```
$ sudo vim /etc/elasticsearch/elasticsearch.yml
# ---------------------------------- Network -----------------------------------
#
# Set the bind address to a specific IP (IPv4 or IPv6):
#
network.host: <bind_ip>
#
# Set a custom port for HTTP:
#
http.port: 9200
#
# For more information, consult the network module documentation.
```
脳死したければ `network.host: "0.0.0.0"`とか入れておけばいいと思う．v6なら `network.host: "::0"`とか？


- 起動してみる．
構築したホスト上で`curl "http://localhost:9200/"` を打つと実際にelasticsearchにアクセスすることができる．
```
$ sudo systemctl start elasticsearch.service
$ sudo systemctl enable elasticsearch.service
$ sudo systemctl status elasticsearch.service
● elasticsearch.service - Elasticsearch
   Loaded: loaded (/usr/lib/systemd/system/elasticsearch.service; disabled; vendor preset: enabled)
   Active: active (running) since Sat 2020-05-16 14:33:17 UTC; 1 day 13h ago
     Docs: https://www.elastic.co
 Main PID: 3301 (java)
    Tasks: 83 (limit: 4915)
   CGroup: /system.slice/elasticsearch.service
           ├─3301 /usr/share/elasticsearch/jdk/bin/java -Xshare:auto -Des.networkaddress.cache.ttl=60 -Des.networkadd
           └─3505 /usr/share/elasticsearch/modules/x-pack-ml/platform/linux-x86_64/bin/controller

May 16 14:33:01 elk01 systemd[1]: Starting Elasticsearch...
May 16 14:33:17 elk01 systemd[1]: Started Elasticsearch.

$ curl "http://localhost:9200/"
{
  "name" : "ela01",
  "cluster_name" : "elc01",
  "cluster_uuid" : "xxxxxxxxxxxxxxxxxxxxxx",
  "version" : {
    "number" : "7.7.0",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "build_date" : "2020-05-12T02:01:37.602180Z",
    "build_snapshot" : false,
    "lucene_version" : "8.5.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

- [つぶやき]: javaをinstallしてないのに動いたなーと思ったらelasticsearchのパッケージを入れるとjavaも一緒に入ってくるっぽい
```
$ /usr/share/elasticsearch/jdk/bin/java --version
openjdk 13.0.2 2020-01-14
OpenJDK Runtime Environment AdoptOpenJDK (build 13.0.2+8)
OpenJDK 64-Bit Server VM AdoptOpenJDK (build 13.0.2+8, mixed mode, sharing)
```
debパッケージをdpkgでいれたりtarballで引っ張ってきて展開したjavaまでついてくるかは知らない．
前はaptでも別にjdk入れないといけなかった気がしたけど，もう忘れてしまった．少なくとも最近のaptからのinstallでは一緒についてくるらしい．
- [Elastic Support Matrix | Elasticsearch](https://www.elastic.co/jp/support/matrix#matrix_jvm)
と思ったらこれのサポートはあくまでelasticsearchを動かすだけにすぎなかった．elasticsearch本体だけなら動くけどlogstashとかはこのjdk verでは動かない．
javaまでのpathが通ってない(and JAVA_HOMEが適切に環境変数に設定されていないと)こんな感じでapt installでerrorを吐く．
```
$ sudo apt install logstash
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
  logstash
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 167 MB of archives.
After this operation, 295 MB of additional disk space will be used.
Get:1 https://artifacts.elastic.co/packages/7.x/apt stable/main amd64 logstash all 1:7.7.0-1 [167 MB]
Fetched 167 MB in 4min 54s (569 kB/s)
Selecting previously unselected package logstash.
(Reading database ... 103575 files and directories currently installed.)
Preparing to unpack .../logstash_1%3a7.7.0-1_all.deb ...
Unpacking logstash (1:7.7.0-1) ...
Setting up logstash (1:7.7.0-1) ...
could not find java; set JAVA_HOME or ensure java is in PATH
chmod: cannot access '/etc/default/logstash': No such file or directory
dpkg: error processing package logstash (--configure):
 installed logstash package post-installation script subprocess returned error exit status 1
```
なのでやっぱりELK stackを構築するなら
`$ sudo apt install openjdk-8-jre`
するのが正解．最近のELKコンポーネントだとopenjdk-11でもいいっぽいけどまぁ好きなの使えばいい気がする．ただjdk9以降はaptとかでさくっと入らないっぽく見えるからめんどそう．

- ref: [OpenJDK: Download and install](https://openjdk.java.net/install/)

### Install logstash and Kibana
```
$ java -version
openjdk version "1.8.0_252"
OpenJDK Runtime Environment (build 1.8.0_252-8u252-b09-1~18.04-b09)
OpenJDK 64-Bit Server VM (build 25.252-b09, mixed mode)
```
javaが入ってる(PATHが通る)ことを確認して
```
sudo apt install logstash
sudo apt install kibana
```
でさくっとはいる．javaへのPATHが通ってないと失敗するので注意．

- kibanaのconfig
```
$ sudo vim /etc/kibana/kibana.yml
server.port: 5601
server.host: "<bind_ip>"
elasticsearch.hosts: ["http://localhost:9200"]
```
<bind_ip>には前述のとおりlistenするipを入れる．

- logstashのconfig
```
$ sudo vim /etc/logstash/jvm.options
-Xms8g
-Xmx8g
```
- 起動
```
$ sudo systemctl start logstash
$ sudo systemctl start kibana
$ sudo systemctl enable logstash
$ sudo systemctl enable kibana

$ sudo systemctl status logstash
● logstash.service - logstash
   Loaded: loaded (/etc/systemd/system/logstash.service; disabled; vendor preset: enabled)
   Active: active (running) since Sat 2020-05-16 15:33:30 UTC; 1 day 11h ago
 Main PID: 7282 (java)
    Tasks: 43 (limit: 4915)
   CGroup: /system.slice/logstash.service
           └─7282 /usr/bin/java -Xms4g -Xmx4g -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseC
...(snip)...

$ sudo systemctl status kibana
● kibana.service - Kibana
   Loaded: loaded (/etc/systemd/system/kibana.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2020-05-16 14:40:41 UTC; 1 day 12h ago
 Main PID: 4569 (node)
    Tasks: 11 (limit: 4915)
   CGroup: /system.slice/kibana.service
           └─4569 /usr/share/kibana/bin/../node/bin/node /usr/share/kibana/bin/../src/cli -c /etc/kibana/kibana.yml
...(snip)...
```

### ex. syslog insert to elasticsearch via logstash
syslogをlogstash経由でelasticsearchにいれてkibanaでみるところまでをやってみる．
まずはsyslog pluginをlogstash-pluginを使って入れる．

- [Working with plugins | Logstash Reference [7.7] | Elastic](https://www.elastic.co/guide/en/logstash/current/working-with-plugins.html)

```
$ /usr/share/logstash/bin/logstash-plugin list syslog
logstash-filter-syslog_pri
logstash-input-syslog
$ /usr/share/logstash/bin/logstash-plugin install logstash-input-syslog
ERROR: File /usr/share/logstash/Gemfile does not exist or is not writable, aborting
$ sudo /usr/share/logstash/bin/logstash-plugin install logstash-input-syslog
Validating logstash-input-syslog
Installing logstash-input-syslog
Installation successful
```

次にlogstashのconfigを作っていく．
適当にsyslogを受ける例を書くとこんな感じ．
```
$ cat /etc/logstash/conf.d/01-syslog.conf
input {
  syslog {
      port => 10514
      type => "syslog"
  }
}

filter {
} # filter

output {
  if "syslog" in [type] {
    elasticsearch {
      hosts => ["localhost:9200"]
      index => "logstash-syslog"
   }
    # stdout { codec => rubydebug }
  }
}
```
ポート番号1024以下はpriviledgedなのでroot権限等がないとpermission deniedで弾かれる．
rootで実行するのも一つの手ではあるが，下記などではport forward(redirect)する例が挙げられている．
今回は10514としてそのまま利用する．

- [Logstash bind to port 514 - Logstash - Discuss the Elastic Stack](https://discuss.elastic.co/t/logstash-bind-to-port-514/44022)

configを書いたらlogstash をrestartする．logstashの嫌なところはこのconfigを変えたらrestartする必要があるところ．
前段にkafkaなどのqueueをいれたりするのがこのrestartによるデータ欠損回避手段のうちの一つかもしれない．
```
$ sudo systemctl restart logstash
```
さて，ここまででlogstash -> elasticsearch ->kibanaのパイプラインが完成した．
実際にlogを突っ込んで見る．
```
logger -n localhost -P 10514 -t mytest -p user.notice --rfc3164 "TEST LOG"
```
これで送られた"はず"である．

ここからはkibanaでみていこう．
kibanaへはconfigした通り，デフォルトでは`5601ポート`でアクセスする．
kibanaでデータを取り扱う上でまずindex-patternを作る必要がある．
Management -> Index Managementでelasticsearch側に先程のsyslogのindexが作成されていることを確認．
これがないとindex patternを作っても見れない．これがない場合elasticsearchに正常にinsertされていない可能性が高いのでまずはそのdebugをしよう．
この画面ではlogstashのconfigでindexとして指定した`logstash-syslog`が見えている．
```eval_rst
.. image:: ../resources/images/elk01.png
```
elasticsearchにindexがあることが確認できたら，今度はkibana側のindex patternsを見る
デフォルトだと何も登録されていないので，`Create Index Pattern`を押す
```eval_rst
.. image:: ../resources/images/elk02.png
```
するとelasticsearchのindexが候補で見えてくる．
```eval_rst
.. image:: ../resources/images/elk03.png
```
index patternの定義をするので，textboxにindex patternの名前(ワイルドカード可)を入れて1つに絞る．
ここでは`logstash-syslog`そのまま入れると一つに絞ることができるので`Next Step`が押せるようになる．
```eval_rst
.. image:: ../resources/images/elk04.png
```
次にtime filter firldを指定する．ここではlogstashでつける`@timestamp`をそのまま使う．
これで`Create Index Pattern`を押す．
```eval_rst
.. image:: ../resources/images/elk05.png
```
するとIndexがつくられて
```eval_rst
.. image:: ../resources/images/elk06.png
```
Index Patternのできあがり．
```eval_rst
.. image:: ../resources/images/elk07.png
```
discover画面に行って確認すると，無事先ほどのloggerコマンドで転送したログが見える．
```eval_rst
.. image:: ../resources/images/elk08.png
```
ここまでで一通りELKスタックを用いてsyslogをkibanaで見えるようになった．

###　まとめ
- Ubuntu18.04にElastic Stack (ELK, Logstash, Elasticsearch, Kibana)をaptパッケージを用いて構築した
- Logstash(log insert) -> Elasticsearch(processiog) <-> Kibana(visualize)のパイプラインにsyslogを投入しkibanaで観測した．

logstashにはこの他にも色々なpluginがあり，様々なデータをELKスタックを用いてデータ分析/可視化することができる  ．
また，logstash以外にもelasticsearchへの出力機能を持ったコンポーネント(ex. fluentd)などが数多くあるので，それらとうまく組み合わせて柔軟な分析基盤の構成ができそうである．

個人的にはDHCPサーバのリースログからクライアントのMACアドレスのベンダコード(OUI)をもとにベンダをを可視化したり，xflow(sflow/netflow)を食わせてgeo-ipの緯度経度データとIPアドレスを突き合わせてどの国/regionとの通信が多いのかを可視化したりすることもやってみたが，なかなかおもしろい．
flowデータをもとにすれば通信をポート番号ベースで可視化したりすることができ，特定ポート宛などのDDoSの検知や内部から外部への不正な大量トラフィック(マルウェア感染端末からのDNS Amp.など)の検出もできそうである．可視化で得られた異常なデータをクリックすることでそのデータの詳細なレコードもkibanaならすぐに見ることができてユーザ体験はとてもよい．
可視化することは人間に直感的にあらゆる物事を短時間で理解させることを手助けする大きな価値のある手法であると思うので，是非このような可視化/分析ツール活用していきたいものである．