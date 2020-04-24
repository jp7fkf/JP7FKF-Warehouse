# Prometheus
- misc of prometheus

## prometheus on docker at ubuntu18.04 host
- [Prometheus Monitoring: Install using Docker - Ubuntu, CentOS - ShellHacks](https://www.shellhacks.com/prometheus-monitoring-install-docker-ubuntu-centos/)
```
sudo mkdir -p /opt/prometheus/{conf,data}
sudo chown 65534:65534 /opt/prometheus/data
```

## prometheus installation on docker
- [Installation | Prometheus](https://prometheus.io/docs/prometheus/latest/installation/)

### snmp monitoring with prometherus
- https://medium.com/@openmohan/snmp-monitoring-and-easing-it-with-prometheus-b157c0a42c0c
- [PrometheusでNW機器のSNMP監視 - Qiita](https://qiita.com/paihu/items/80a95b2eaf3b17a921af)
- [prometheusのsnmp exporterをdockerで動かす](https://hakengineer.xyz/2018/05/16/post-1202/)

#### snmp-expoerterをcurlで叩く　
- `curl -s '<snmp-exporter-ip>:9116/snmp?target=x.x.x.x&module=cisco'`

### prometheus misc
- https://kazuhira-r.hatenablog.com/entry/2019/02/11/205455
- [Prometheus 使ってみたメモ - ngyukiの日記](https://ngyuki.hatenablog.com/entry/2017/11/14/203554)
- [Comparison to alternatives | Prometheus](https://prometheus.io/docs/introduction/comparison/)

## grafana
- scripted dashboardでdashboardをcodeで管理するのがあたまよさそう．

## loki
- [Grafana Loki | Grafana Labs](https://grafana.com/oss/loki/)
- canaryの仕組みは参考になる．ログ収集のシステムがこわれてないか監視することは大切．サービス監視の一種として見て取れる．
- 現時点(201912)では，prometheusのアーキテクチャが容易に導入できるようにconteinerizedされたサービスには比較的導入が容易そうであり，この場合はmetricsで収集したデータから，気になる箇所のログを見たいときに同一の時刻範囲，labelがつけられたログに飛べるので便利そうである．
- ただ，prometheusのアーキテクチャにストレスレスに乗っかれないシステム等では，これでlogをみるメリットはそう多くなさそうに思える．素直にelastic stack等でみてあげたほうが良さそうに思える．
