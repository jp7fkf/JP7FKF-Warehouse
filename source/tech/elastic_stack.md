# Elastic Stack
- Elasticsearch, Logstash, Kibana, Beatsなどのあれこれ．

## Elasticsearch
### dump
- レポジトリを作ってデータバックアップ，リストア
  - http://togattti.hateblo.jp/entry/2016/11/09/120710
  - https://cloudpack.media/7139
- elasticdumpを使う方法
  - https://qiita.com/nakazii-co-jp/items/3199433d685d0600c6d6
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
- https://www.elastic.co/guide/en/logstash/6.2/plugins-filters-geoip.html
- geolite.maxmind.com
- http://kodamap.hatenablog.com/entry/2017/07/11/230705
- https://dev.maxmind.com/geoip/geoip2/geolite2/

#### dns
- https://www.elastic.co/guide/en/logstash/current/plugins-filters-dns.html

#### flow
- http://enog.jp/wp-content/uploads/2015/09/enog43_elk_0904.pdf
  - ASN, gio-ipはいれたい．
- https://www.janog.gr.jp/meeting/janog39/application/files/7014/8481/0318/janog39-traffic-nishizuka-03.pdf

#### slack通知
  - [logstash-plugins/logstash-output-slack](https://github.com/logstash-plugins/logstash-output-slack)
  - https://github.com/cyli/logstash-output-slack/issues/24

- こんな分岐のしかたもある．
```
if [dstip] and [dstip] !~ "(^127.0.0.1)|(^10.)|(^172.1[6-9].)|(^172.2[0-9].)|(^172.3[0-1].)|(^192.168.)|(^169.254.)" {
```

### Tools
- grok debugger
  - https://grokdebug.herokuapp.com/
- elastic search tips
  - kuromoji tokenizer: http://pppurple.hatenablog.com/entry/2017/05/28/141143

### Others
- Multiple Pipeline
  - https://qiita.com/micci184/items/24e197a168891f089b3d

- 振り分け系
  - https://designetwork.daichi703n.com/entry/2017/04/10/logstash-multiple-output

- flow on ELK
  - http://enog.jp/wp-content/uploads/2015/09/enog43_elk_0904.pdf
  - [sflow | Logstash Reference [5.2] | Elastic](https://www.elastic.co/guide/en/logstash/5.2/plugins-codecs-sflow.html)

- netflow module
  - https://www.elastic.co/guide/en/logstash/current/netflow-module.html

- logstash 入門
  - https://qiita.com/nihei9/items/1ebc7934eb1e1ce66162

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
- installation: https://www.elastic.co/guide/jp/kibana/current/deb.html
- installation to ubuntu18.04: https://qiita.com/hogehogehugahuga/items/e4ce03269309e90d04b8
- https://thinkit.co.jp/article/13444
- https://www.n-novice.com/entry/2018/02/22/214421#logstashインストール
- beats: https://dev.classmethod.jp/server-side/elasticsearch/beats-entry-matome/
- metricbeat: https://qiita.com/hana_shin/items/95cf8e165333f2a9d1c1
- kibanaの正規表現がいけてない．普通のregexpでは反応しない．
- visualize of netflow: http://kodamap.hatenablog.com/entry/2017/07/11/230705
- log aggregator with logstash: http://success.tracpath.com/blog/2014/06/04/logstash%E3%81%AB%E3%82%88%E3%82%8B%E3%83%AD%E3%82%B0%E5%8F%8E%E9%9B%86%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0%E3%81%AE%E6%A7%8B%E7%AF%89/
- https://knowledge.sakura.ad.jp/2736/
- https://support.halon.io/hc/en-us/articles/360000700065-Remote-syslog-to-Logstash
- https://qiita.com/quotto/items/8250c67ced43dc83b770
- https://protocol.nekono.tokyo/2018/11/01/logstashで複数ポートでsyslog転送を受け付ける/
- // 結局groksを上手に書いて多数のログ形式をサポートするのがつらい．

- installation logstash: https://symfoware.blog.fc2.com/blog-entry-1910.html
- the first logstash: https://qiita.com/nskydiving/items/0cb598de7ffb5c22424d

- syslog: serverity, rsyslog, logger command
- geoip with logstash: https://www.digitalocean.com/community/tutorials/how-to-map-user-location-with-geoip-and-elk-elasticsearch-logstash-and-kibana
- logstash grok: https://qiita.com/tuneyukkie/items/75cbb4d44f901fec2188
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