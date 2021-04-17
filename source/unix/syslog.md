# syslog

## basic
- IETF，BSDが主なフォーマット
  - [RFC 3164 - The BSD Syslog Protocol](https://tools.ietf.org/html/rfc3164)
    - legacy formatとも呼ばれる．
  - [RFC 5424 - The Syslog Protocol](https://tools.ietf.org/html/rfc5424)
    - BSDを拡張したもの．
- syslog messageの中身については特に規定がないが，csvだったりCEFだったりいろいろある．

## rsyslog
- [GitHub - rsyslog/rsyslog: a Rocket-fast SYStem for LOG processing](https://github.com/rsyslog/rsyslog)
- [rsyslogでシステムログ管理 - Qiita](https://qiita.com/tasakii/items/10db42392c487d5276da)
- [rsyslogの最低限の設定 – おぼえがきつめあわせ](http://www.no1497.com/?p=326)

### configuration
#### `/etc/rsyslog.d/hoge.conf` でdiscard
- その下にはログは行かない．つまり，一番下に書いても意味はない．順序大事．
```
#discard loadbalancer messages
:msg, contains, "[disrard_regexp]" ~
```

#### rsyslogのproperties: [rsyslog Properties — rsyslog 8.1911.0 documentation](https://www.rsyslog.com/doc/v8-stable/configuration/properties.html)
```
$template MyTemplate,
"%TIMESTAMP:::date-rfc3339%|%syslogpriority-text%|%HOSTNAME%|%msg:::sp-if-no-1st-sp%%msg:::drop-last-lf%\n"
$ActionFileDefaultTemplate MyTemplate
```
#### rsyslogでホストごとに
- `%hostname%`: ログのhostname fieldに入っているhostname
- `%fromhost-ip%`: 送信元IP
- `%fromhost%`: 送信元IPを逆引きしたhostname

この辺のプロパティを利用して分ける．
- ex.)
```
$template logFileName,"/var/log/syslog/%hostname%/%$year%%$month%%$day%.log"

if $fromhost-ip == ['192.168.0.1' , '192.168.0.2' ] then {
  /var/log/specified_ips.log
  stop
}
# & ~で止めるのは2021/04現在推奨されていない．かわりにstopを使う．"& stop"とか．

```


## syslogをテスト的に投げたいperl
- [リモートsyslog送信スクリプト - mikedaの日記](https://mikeda.hatenablog.com/entry/20101123/1290529356)
- 結局logger commandでいい気がする．

## facility
A facility code is used to specify the type of program that is logging the message. Messages with different facilities may be handled differently.[5] The list of facilities available is defined by the standard:[2]:9
```
+----------+-----------------+------------------------------------------+
| Facility |                 |                                          |
|     code | Keyword         | Description                              |
+----------+-----------------+------------------------------------------+
|        0 | kern            | Kernel messages                          |
|        1 | user            | User-level messages                      |
|        2 | mail            | Mail system                              |
|        3 | daemon          | System daemons                           |
|        4 | auth            | Security/authentication messages         |
|        5 | syslog          | Messages generated internally by syslogd |
|        6 | lpr             | Line printer subsystem                   |
|        7 | news            | Network news subsystem                   |
|        8 | uucp            | UUCP subsystem                           |
|        9 | cron            | Clock daemon                             |
|       10 | authpriv        | Security/authentication messages         |
|       11 | ftp             | FTP daemon                               |
|       12 | ntp             | NTP subsystem                            |
|       13 | security        | Log audit                                |
|       14 | console         | Log alert                                |
|       15 | solaris-cron    | Scheduling daemon                        |
|    16–23 | local0 – local7 | Locally used facilities                  |
+----------+-----------------+------------------------------------------+
```
The mapping between facility code and keyword is not uniform in different operating systems and syslog implementations.[6]

## Severity level
The list of severities is also defined by the standard:[2]:10
```
+-------+---------------+---------+-----------------------------------+-------------------------------------------------------------------------------------------+
| Value | Severity      | Keyword | Description                       | Condition                                                                                 |
+-------+---------------+---------+-----------------------------------+-------------------------------------------------------------------------------------------+
|     0 | Emergency     | emerg   | System is unusable                | A panic condition.[8]                                                                     |
|     1 | Alert         | alert   | Action must be taken immediately  | A condition that should be corrected immediately, such as a corrupted system database.[8] |
|     2 | Critical      | crit    | Critical conditions               | Hard device errors.[8]                                                                    |
|     3 | Error         | err     | Error conditions                  |                                                                                           |
|     4 | Warning       | warning | Warning conditions                |                                                                                           |
|     5 | Notice        | notice  | Normal but significant conditions | Conditions that are not error conditions, but that may require special handling.[8]       |
|     6 | Informational | info    | Informational messages            |                                                                                           |
|     7 | Debug         | debug   | Debug-level messages              | Messages that contain information normally of use only when debugging a program.[8]       |
+-------+---------------+---------+-----------------------------------+-------------------------------------------------------------------------------------------+
```
- ref: [syslog - Wikipedia](https://en.wikipedia.org/wiki/Syslog)
