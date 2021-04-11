# systemctl

## example
- `/etc/systemd/system/` 配下とかに配置．
```
[Unit]
Description=docker-compose nginx-proxy service
Requires=docker.service

[Service]
User=root
Type=simple

Environment=COMPOSE_FILE=/home/composer/nginx-proxy/docker-compose.yml

ExecStartPre=/usr/bin/docker-compose -f ${COMPOSE_FILE} kill
ExecStartPre=/usr/bin/docker-compose -f ${COMPOSE_FILE} rm -v -f
ExecStartPre=/usr/bin/docker-compose -f ${COMPOSE_FILE} down

ExecStart=/usr/bin/docker-compose -f ${COMPOSE_FILE} up --force-recreate --abort-on-container-exit

ExecStop=/usr/bin/docker-compose -f ${COMPOSE_FILE} stop
ExecStopPost=/usr/bin/docker-compose -f ${COMPOSE_FILE} rm -v -f
ExecStopPost=/usr/bin/docker-compose -f ${COMPOSE_FILE} down

Restart=always
RestartSec=180s

[Install]
WantedBy=multi-user.target
```

### unit section
```
Description: 説明
Documentation: ドキュメント
Requires: 自Unitが必要とする前提Unit(依存unit起動に失敗した場合，自unitも失敗する．強い依存．)
Wants: 自Unitが必要とする前提Unit(依存unitが失敗しても，自unitは起動を継続する．弱い依存．)
After: 自Unitより先に起動するべきUnit
Before: 自Unitより後に起動するべきUnit
Conflict: 自Unitと同時に起動するべきでないUnit
```
- Requires/WantsとAfter/Beforeは概念として異なるものだということに注意する．依存関係と起動順序は異なるものである．

### install section
```
WantedBy: enable時にこのUnitの.wantsディレクトリにリンクを作成する
RequiredBy: enable時にこのUnitの.requiredディレクトリにリンクを作成する
Also: enable/disable時に同時にenable/disableするUnit
Alias: enable時にこのUnitの別名を用意
```
- ランレベルとtargetの対応
  - 0: poweroff.target(電源停止)
  - 1: rescue.target(レスキュー/シングルユーザモード)
  - 3: multi-user.target(マルチユーザモード(console))
  - 5: graphical.target(GUIモード)
  - 6: reboot.target(再起動)
  - none: emergency.target(緊急モード)

### service
```
User: Unitを操作するユーザ
Group: Unitを操作するグループ
RootDirectory: 指定directoryにchrootする．
ExecStart: 起動コマンド
ExecReload: reloadコマンド
ExecStop: 停止コマンド
ExecStartPre/ExecStartPost: サービス起動前後の追加コマンド（サービス起動判定には関連させたくないコマンドを記載）
ExecStopPost: サービス停止後に実行するコマンド（サービスが異常停止した際にも実行される）
EnvironmentFile: 環境変数を読み込むファイル
Type: サービスプロセスの起動完了の判定法(simple[default]/forking/oneshot/notify/dbus)
RemainAfterExit: oneshotの場合等，exitしたあともstatus activeを継続したい場合にyesとする(yes/no)
PIDFile: fork型サービスのメインプロセスのPIDファイル指定(forkするとmainプロセスpidが掌握できないため，メインプロセスPIDを示すファイルを指定する．)
BusName: D-Bus型サービスのbus接続名
Restart: サービスプロセス停止時の再起動条件(no[default]/always/on-success/on-failure)
RestartSec: リスタートを試みる間隔
StartLimitInterval: 起動制限インターバル．インターバル時間中にStartLimitBurst回を超えた回数startすると制限される．
StartLimitBurst: 起動制限回数
PrivateTmp:  このサービス専用の/tmpと/var/tmpを用意する
KillMode: ExecStopでプロセスが残っている場合にどうするか．(none(残置)/process(メインプロセスをsigterm/sigkill)/control-group(グループ内全プロセスをsigterm/sigkill)/mixed(processしてcontrol-groupする))
```

## commands
- `systemctl [status/enable/disable/start/stop/reload/reload-or-restart/try-restart/reload-or-try-restart] example.service`
  - `reload`はunitがreload対応しているときのみ．
  - `try-restart`系は停止している場合は何もしない，`restart`の場合は停止しても起動する．
  - `reload-or-*`系はreload対応していればreload，そうでない場合は`reload`, or `try-reload`．
- `systemctl daemon-reload`
  - unitファイル再読み込み．
- `systemctl list-unit-files --type=service`
  - `type=service`一覧
- `systemctl is-enabled example.unit`

## References
- [Systemd入門(4) - serviceタイプUnitの設定ファイル - めもめも](https://enakai00.hatenablog.com/entry/20130917/1379374797)
