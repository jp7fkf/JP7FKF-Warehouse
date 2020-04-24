# python

## validation
- yaml validation
  - [Validating a yaml document in python - Stack Overflow](https://stackoverflow.com/questions/3262569/validating-a-yaml-document-in-python)

## netaddr
### ip範囲からCIDRを求める
- `netaddr.iprange_to_cidrs` が便利

- たとえば
`ips_tuple = ({"start_ip": "192.168.1.0", "end_ip": "192.168.1.127"}, {"start_ip": "192.168.100.128", "end_ip": "192.168.100.255"}, )`
的なタプルがあったとして，cidr表記を求めたいとすると，
```
cidrs = []
for ipaddress in ips_tuple:
    cidrs.append(*netaddr.iprange_to_cidrs(ipaddress["start_ip"], ipaddress["end_ip"]))
cidrs = list(map(lambda cidr: str(cidr), cidrs))
```
するだけでcidrsにはstrで `['192.168.1.0/25', '192.168.100.128/25']` が入るなどする．

- ex.
```
$ python3
Python 3.6.5 (default, Jun  4 2018, 09:18:59)
[GCC 4.2.1 Compatible Apple LLVM 9.1.0 (clang-902.0.39.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import netaddr
>>> ips_tuple = ({"start_ip": "192.168.1.0", "end_ip": "192.168.1.127"}, {"start_ip": "192.168.100.128", "end_ip": "192.168.100.255"}, )
>>> cidrs = []
>>> for ipaddress in ips_tuple:
...   cidrs.append(*netaddr.iprange_to_cidrs(ipaddress["start_ip"], ipaddress["end_ip"]))
...
>>> cidrs = list(map(lambda cidr: str(cidr), cidrs))
>>> print(cidrs)
['192.168.1.0/25', '192.168.100.128/25']
>>>
```

## aplayを使ってpythonでsound 再生をする
- os.popen('aplay --device=plughw:CARD=PCH,DEV=7 /home/jp7fkf/nc106374.wav')

## パッケージ化する
- pipでinstallとかができるようになる．
- [Python: 自作パッケージにデータファイルを含める - CUBE SUGAR CONTAINER](https://blog.amedama.jp/entry/2015/12/26/012332)

## journaldにloggingする
systemd serviceとして動かしておけばstderr/stdoutが勝手にjournaldに入ってloggingされる．
pythonの場合,python-systemd, systemd.journald.send()を使うとより詳細にlog出力が可能
- ref: [python から journal にログを書き出す - memo](https://u7fa9.org/memo/HEAD/archives/2015-11/2015-11-21.rst)

```
# pyhon3
import sys
sys.stdout.write('Dive in')
sys.stderr.write('Error occurred!')

# or

print('Error: configuration failed', file=sys.stderr)
```

## Tips
-  [Pythonの相対インポートで上位ディレクトリ・サブディレクトリを指定 | note.nkmk.me](https://note.nkmk.me/python-relative-import/)

## systemdにpythonスクリプトをservice登録してデーモン化する

- service fileを書く
```
% cat /etc/systemd/system/fkfbot.service
[Unit]
Description = fkfbot
After=network.target network-online.target

[Service]
ExecStart = /home/jp7fkf/fkfbot/run.py
Restart = always
RestartSec=30
Type = simple

[Install]
WantedBy = multi-user.target
```
- おまじない
```
% sudo systemctl daemon-reload
```
- 読まれた．適宜 `sudo systemctl enable testscript_py` やらをすればいい
```
[jp7fkf@lab1 18:11:52] ~
% systemctl status testscript_py
● testscript_py.service - testscript_py
   Loaded: loaded (/etc/systemd/system/testscript_py.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2020-03-04 00:47:02 JST; 17h ago
 Main PID: 1857 (python3)
    Tasks: 12 (limit: 4434)
   CGroup: /system.slice/testscript_py.service
           └─1857 /home/jp7fkf/.pyenv/versions/3.8.1/bin/python3 /home/jp7fkf/testscript_py/run.py
```

## Tips
-  [Pythonの相対インポートで上位ディレクトリ・サブディレクトリを指定 | note.nkmk.me](https://note.nkmk.me/python-relative-import/)
