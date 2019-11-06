# syslog

## rsyslog
- https://qiita.com/tasakii/items/10db42392c487d5276da
- http://www.no1497.com/?p=326

## `/etc/rsyslog.d/hoge.conf` でdiscard
- その下にはログは行かない．つまり，一番下に書いても意味はない．順序大事．
```
#discard loadbalancer messages
:msg, contains, "[disrard_regexp]" ~
```

## syslogをテスト的に投げたいperl
https://mikeda.hatenablog.com/entry/20101123/1290529356
- 結局logger commandでいい気がする．

## rsyslogのproperties: https://www.rsyslog.com/doc/v8-stable/configuration/properties.html
```
$template MyTemplate,
"%TIMESTAMP:::date-rfc3339%|%syslogpriority-text%|%HOSTNAME%|%msg:::sp-if-no-1st-sp%%msg:::drop-last-lf%\n"
$ActionFileDefaultTemplate MyTemplate
```