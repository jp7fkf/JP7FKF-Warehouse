# MYSQL

## `Statement is unsafe because it uses a system function that may return a different value on the slave`
  - `Statement is unsafe because it uses a system function that may return a different value on the slave`
  - ` [Warning] Unsafe statement written to the binary log using statement format since BINLOG_FORMAT = STATEMENT. INSERT... ON DUPLICATE KEY UPDATE  on a table with more than one UNIQUE KEY is unsafe Statement: <statement>`

  - バイナリログのフォーマット的に，ステートメントのロギングをしてしまっていてwarningが出るやつ．
  - `SELECT @@binlog_format;` をしてみると今のロギングフォーマットがわかる．
  - `select variable_value as "BINARY LOG FORMAT (binlog_format) :: " from information_schema.global_variables where variable_name='binlog_format';`でも可．
  - loggin format は下記の3種類
    - RAW: ステートメントの実行結果をbinary logとして保存
    - STATEMENT: ステートメント自体をbinary logとして保存
    - MIXED: 危険なステートメントをRAWとして，そのほかはステートメントをbinary logとして保存
  - 危険なステートメント，とは，`sys_date()` 的な関数がステートメントに入っているやつ．
  - レプリケーションを設定していると，master/slaveで実行結果が異なる可能性があるのでwarningが出るようだ．
  - warningを止める策の1つとして，binary log format を`mixed` もしくは`raw` にしてしまうという手がありそう．(`my.cnf` の`binlog_format` 行を書き換える)
  - [MySQL :: MySQL 5.6 Reference Manual :: 17.1.2.3 Determination of Safe and Unsafe Statements in Binary Logging](https://dev.mysql.com/doc/refman/5.6/en/replication-rbr-safe-unsafe.html)
  - [日々の覚書: MySQLのNOW関数はどのようにして安全にスレーブでリプレイされるのか](https://yoku0825.blogspot.com/2016/12/mysqlnow.html)
