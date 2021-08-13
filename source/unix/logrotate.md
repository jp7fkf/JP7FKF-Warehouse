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
- pre/postrotateではrotate時の前後にshell script形式で実行したいactionを記述できる．
- create指定ではrotate後にfileを生成できる．(inode番号が変わったりはあるが，ファイルが存在しないことによるデーモンのクラッシュ等はある程度抑制できる)

## Tips
- logrotateでinode番号が変わる/なくなることにより動作中のprocessが正常に書き込みが行えなくなる可能性がある．
  - 影響のあるプロセスがわかっている場合，対象のプロセスに対してSIGHUPを送るなどして新しいinodeで掴み直してもらうなどの対処が可能．
  - 上記が不可能な場合はcopytruncateを用いることでinode番号は変えずにrotateする際にファイルの中身を移植しinodeを保持することができる．
    - ただしいfdの開き方や操作中のdaemonなどによってはseekが保持され書き込み位置が想定外になったり思わぬハマり方をする可能性もある．
    - そもそも移植はマシンの負荷コストもそれなりに高いのでできれば避けたいものである．

## References
- [【logrotate】の仕組みと書き方, オプション一覧, 設定反映と再起動について | SEの道標](https://milestone-of-se.nesuke.com/sv-basic/linux-basic/logrotate/)
- [log rotation まわりの話 | 1Q77](https://blog.1q77.com/2013/02/log-rotation-%E3%81%BE%E3%82%8F%E3%82%8A%E3%81%AE%E8%A9%B1/)
