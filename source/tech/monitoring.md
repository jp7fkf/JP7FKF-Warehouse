# Monitoring
- サービス，サーバ，ネットワークの監視は奥が深い．

## 監視系Tool
### NW, Server系
- [Zabbix](https://www.zabbix.com/)
- [Cacti](https://www.cacti.net/)
- [MUNIN](http://munin-monitoring.org/)
- [MRTG](https://oss.oetiker.ch/mrtg/)

### App系
- [Datadog](https://www.datadoghq.com/)

### ログ監視
- [Fluentd](https://www.fluentd.org/)
- [swatch](https://sourceforge.net/projects/swatch/)
- [graylog](https://www.graylog.org/)
  - [Graylog2/graylog2-server: Free and open source log management](https://github.com/Graylog2/graylog2-server)
  - 裏側elasticsearchでいけるのでよさそう．
  - logを受けるフロントとGUI，アラートがセットになっている．受けたログは裏のelasticsearchにながす．
- [rsyslog](https://www.rsyslog.com/)
- [Splunk](https://www.splunk.com/)

### 可視化系
- [grafana](https://grafana.com/)
- [kibana](https://www.elastic.co/jp/products/kibana)

### チケットシステム系
- [PagerDuty](https://www.pagerduty.com/)
- [Service Now](https://www.servicenow.com/)
- [GitHub - Netflix/dispatch: All of the ad-hoc things you're doing to manage incidents today, done for you, and much more!](https://github.com/Netflix/dispatch)
  - netflixがOSS化したtool．危機管理系．jiraやpagerdutyなどの各種saas等とも連携可能らしい．

### Flow
- [GitHub - pmacct/pmacct: pmacct is a small set of multi-purpose passive network monitoring tools [NetFlow IPFIX sFlow libpcap BGP BMP RPKI IGP Streaming Telemetry].](https://github.com/pmacct/pmacct)

### Others (わからんのはとりあえずここに入れられる結末)
- [Mackerel](https://mackerel.io/ja/)
- [ThousandEyes](https://www.thousandeyes.com/)
- [Prometheus](https://prometheus.io/)
- [Nagios](https://www.nagios.org/)
- [Ichinga 2](https://icinga.com/docs/icinga2/latest/)
- [Hinemos](https://www.hinemos.info/hinemos)
- [Pandora FMS](http://pandorafms.org/ja/)
- [collectd](https://collectd.org/)
- [Elastic](https://www.elastic.co/jp/)
  - logstashとか，elasticsearchとか，kibanaとか．
  - ELKとか言いますよね．elasticstackとか．
- [fping](https://fping.org/)
  - 死活監視
- [Hatohol](http://www.hatohol.org/)
- [Sensu](https://sensu.io/)
- [New Relic](https://newrelic.com/)
- [Re:dash](https://redash.io/)
  - ダッシュボードツール
- [TICK Stack](https://www.influxdata.com/time-series-platform/)
- [Xymon](http://xymon.sourceforge.net/)
- [GitHub - netdata/netdata: Real-time performance monitoring, done right! https://my-netdata.io/](https://github.com/netdata/netdata)
  - リアルタイム性に優れる
- [Telegraf Open Source Server Agent | InfluxData](https://www.influxdata.com/time-series-platform/telegraf/)
  - [GitHub - influxdata/telegraf: The plugin-driven server agent for collecting & reporting metrics.](https://github.com/influxdata/telegraf)
- [Netdata](https://www.netdata.cloud/)
- [GitHub - VictoriaMetrics/VictoriaMetrics: VictoriaMetrics: fast, cost-effective monitoring solution and time series database](https://github.com/VictoriaMetrics/VictoriaMetrics)
- [GitHub - thanos-io/thanos: Highly available Prometheus setup with long term storage capabilities. A CNCF Incubating project.](https://github.com/thanos-io/thanos)
- [GitHub - open-fresh/avalanche: Prometheus/OpenMetrics endpoint series generator for load testing.](https://github.com/open-fresh/avalanche)
- [Welcome to Apache Solr - Apache Solr](https://solr.apache.org/)
- [StackStorm - StackStorm](https://stackstorm.com/)
  - イベントドリブンな自動化ツール
- [Amixr Incident Management for DevOps and SRE inside Slack](https://amixr.io/)
- [GridDB Developers](https://griddb.net/ja/)
- [GitHub - perfsonar/owamp: A tool for performing one-way or two-way active measurements](https://github.com/perfsonar/owamp)
- [システム運用管理ならNRIのSenju Family](https://senjufamily.nri.co.jp/)
- [統合運用管理ソフトウェア WebSAM: ソフトウェア | NEC](https://jpn.nec.com/websam/index.html)
- [Hinemosとは | Hinemos](https://www.hinemos.info/hinemos)
- [統合システム運用管理 JP1：日立](https://www.hitachi.co.jp/Prod/comp/soft1/jp1/index.html)
- [統合運用管理 Systemwalker｜ソフトウェア : 富士通](https://www.fujitsu.com/jp/products/software/middleware/business-middleware/systemwalker/)
- [GitHub - theonedev/onedev: Self-hosted Git Server with CI/CD and Kanban](https://github.com/theonedev/onedev)
- [GitHub - cisagov/decider: A web application that assists network defenders, analysts, and researchers in the process of mapping adversary behaviors to the MITRE ATT&CK® framework.](https://github.com/cisagov/Decider/)

### messaging
- [XMPP | XMPP Main](https://xmpp.org/)

## ref
- [OSSのおすすめ監視サーバ・監視ツール比較17選 | OSSでのシステム構築・デージーネット](https://www.designet.co.jp/ossinfo/selection/monitoring-server.html)
- [手軽なメトリクス視覚化ツールを探して (Netdata, Prometheus, Telegraf) - Qiita](https://qiita.com/kikuchi_kentaro/items/8ab93b73012ee8aea717)
- [GitHub - kahun/awesome-sysadmin: A curated list of amazingly awesome open source sysadmin resources inspired by Awesome PHP.](https://github.com/kahun/awesome-sysadmin)

## 課題とかアイディア
- 階層型の監視システム？
- 監視システム自体の監視
