# elastiflow

## elastiflow install battle
- [elastiflow/INSTALL.md at master · robcowart/elastiflow · GitHub](https://github.com/robcowart/elastiflow/blob/master/INSTALL.md)
  - これに乗っ取るほかない．
- [ElastiFlowでNetFlow(Frotigate)を視覚化 - 備忘録／にわかエンジニアが好きなように書く](https://www.n-novice.com/entry/2020/05/06/000000)

```
jp7fkf@elk01:~$ /usr/share/logstash/bin/logstash-plugin install logstash-codec-sflow
ERROR: File /usr/share/logstash/Gemfile does not exist or is not writable, aborting
jp7fkf@elk01:~$ sudo /usr/share/logstash/bin/logstash-plugin install logstash-codec-sflow
Validating logstash-codec-sflow
Installing logstash-codec-sflow
Installation successful
jp7fkf@elk01:~$ sudo /usr/share/logstash/bin/logstash-plugin update logstash-codec-netflow
Updating logstash-codec-netflow
No plugin updated
jp7fkf@elk01:~$ sudo /usr/share/logstash/bin/logstash-plugin update logstash-input-udp
Updating logstash-input-udp
Updated logstash-input-udp 3.3.4 to 3.4.1
jp7fkf@elk01:~$ sudo /usr/share/logstash/bin/logstash-plugin update logstash-input-tcp
Updating logstash-input-tcp
Updated logstash-input-tcp 6.0.4 to 6.0.9
jp7fkf@elk01:~$ sudo /usr/share/logstash/bin/logstash-plugin update logstash-filter-dns
Updating logstash-filter-dns
No plugin updated
jp7fkf@elk01:~$ sudo /usr/share/logstash/bin/logstash-plugin update logstash-filter-geoip
Updating logstash-filter-geoip
Updated logstash-filter-geoip 6.0.3 to 7.1.0
jp7fkf@elk01:~$ sudo /usr/share/logstash/bin/logstash-plugin update logstash-filter-translate
Updating logstash-filter-translate
No plugin updated
jp7fkf@elk01:~$

jp7fkf@elk01:~/elastiflow/elastiflow-master/logstash$ sudo cp -r elastiflow/ /etc/logstash/
jp7fkf@elk01:~/elastiflow/elastiflow-master/logstash$ ls /etc/log
ls: cannot access '/etc/log': No such file or directory
jp7fkf@elk01:~/elastiflow/elastiflow-master/logstash$ ls /etc/logstash/
01-syslog.conf.bak  elastiflow   log4j2.properties     logstash.yml   startup.options
conf.d              jvm.options  logstash-sample.conf  pipelines.yml
jp7fkf@elk01:~/elastiflow/elastiflow-master/logstash$ ls -la /etc/logstash/
total 60
drwxrwxr-x  4 root root 4096 Apr 15 11:07 .
drwxr-xr-x 99 root root 4096 Apr 15 11:06 ..
-rw-r--r--  1 root root  687 May 18  2020 01-syslog.conf.bak
drwxrwxr-x  3 root root 4096 May 18  2020 conf.d
drwxr-xr-x  8 root root 4096 Apr 15 11:07 elastiflow
-rw-r--r--  1 root root 2019 May 16  2020 jvm.options
-rw-r--r--  1 root root 8958 May 12  2020 log4j2.properties
-rw-r--r--  1 root root  342 May 12  2020 logstash-sample.conf
-rw-r--r--  1 root root 9475 May 16  2020 logstash.yml
-rw-r--r--  1 root root  285 May 12  2020 pipelines.yml
-rw-------  1 root root 1696 May 12  2020 startup.options

jp7fkf@elk01:~/elastiflow/elastiflow-master$ sudo mkdir /etc/systemd/system/logstash.service.d/
jp7fkf@elk01:~/elastiflow/elastiflow-master$ sudo cp -r logstash.service.d/elastiflow.conf /etc/systemd/system/logstash.service.d/
jp7fkf@elk01:~/elastiflow/elastiflow-master$ sudo less /etc/systemd/system/logstash.service.d/elastiflow.conf
jp7fkf@elk01:~/elastiflow/elastiflow-master$ sudo systemctl daemon-reload
```