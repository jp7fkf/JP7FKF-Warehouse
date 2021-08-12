# logrotete

## basic
- `/etc/logrotate.conf`
- `/etc/logrotate.d/test`
- ex. `/etc/logrotate.d/test`
```
/var/log/test.log {
    weekly
    rotate 5
    compress
    delaycompress
    missingok
    notifempty
}
```

## いろいろ
```
/var/log/td-agent/test2.log {
  daily
  maxsize 100M
  rotate 30
  compress
  delaycompress
  notifempty
  create 640 td-agent td-agent
  sharedscripts
  postrotate
    pid=/var/run/td-agent/td-agent.pid
    if [ -s "$pid" ]
    then
      kill -USR1 "$(cat $pid)"
    fi
  endscript
}
```
- maxsize指定でそのサイズを超えたときに時間指定のタイミングを待たずrotateを走らせることができる．
- pre/postrotateではrotate時の前後にshell script形式で実行したいactionを記述できる
- create指定ではrotate後にfileを生成できる．(inode番号が変わったりはあるが，ファイルが存在しないことによるデーモンのクラッシュ等はある程度抑制できる．)