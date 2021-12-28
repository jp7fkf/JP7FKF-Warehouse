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

## error handlingとtraceのプラクティス
### methodの戻り値
- 返り値の第1要素はそのmethodから得られる活用される値．第2要素はerror要素であり，code, message, traceを含むように返す．
  - 正常応答の場合はerror要素はNoneとすることで成功か失敗かを判定する(-> goっぽい感じ)
  - error要素がnoneの場合はそのメソッドから得たい値(返り値の第1要素)はそのメソッドが返す正常応答値を返却する．正常応答値はあらゆる値をとりうる(None可)．
  - traceにはそのmethodが他のmethodをcallし，errorを受けた場合にcall先のerrorを内包させる．これによってroot callerまで末端のerrorを浮かびあがらせることができ，debugが用意になり，testも単純になる．
    - error発生点が自methodの場合はtraceは空arrayとすればよい
```
return (None, {"code": -1, "message": "unexpected error.", "trace": []})
return (None, {"code": -1, "message": "unexpected error.", "trace": [_err]+_err.pop("trace")})
```
- error要素に含まれるcodeやmessageは任意とすることができる．
  - codeはunit testのassertionや，debug用としてerorr箇所を一意に定めるために活用することができる．
    - 当初汎用errorを-1とする．
    - HTTP errorの場合などはHTTP status codeをそのまま挿入することも有効である．
  - messageは基本的に自method名を含めるとtraceに挿入されたときにたどることが容易となる．
    - 複数のerror点がある場合はカッコ書き等で詳細を補足するとerror箇所の特定が容易となる．
```
return (None, {"code": -1, "message": "unexpected error(unexpected exception).", "trace": []})
return (None, {"code": -1, "message": "unexpected error(xx api error).", "trace": [_err]+_err.pop("trace")})
```

### mockを用いたunittest
- mockを用いたtestはテスト対象が他のmethodをcallする部分をmockすることを基本とする．
- callするmethod先で用いられているmockをapplyすることは極力避けるが，例外は存在する．
  - methodの繰り返し呼び出しがありうるためmock箇所の断定が難しくなるなどの理由による．
  - 影響範囲が小さく断定が容易な場合などは例外としてもよいが，極力避ける．

## asyncio.gatherのexception handling
```
>>> async def waiting_for_hoge(hoge=None):
>>>     print("start")
>>>     await asyncio.sleep(1)
>>>     print(hoge[0])
>>>     print("end")
>>>
>>> results = await asyncio.gather(*[waiting_for_hoge(hoge=["1"]),
>>>                                 waiting_for_hoge(hoge=None),
>>>                                 waiting_for_hoge(hoge=["3"])],
>>>                                 return_exceptions=True)
... print(results)
start
start
start
1
end
3
end
[None, TypeError("'NoneType' object is not subscriptable"), None]
>>>

>>> results = await asyncio.gather(*[waiting_for_hoge(hoge=["1"]),
>>>                                  waiting_for_hoge(hoge=None),
>>>                                  waiting_for_hoge(hoge=["3"])],
>>>                                return_exceptions=False)
... print(results)
...
start
start
start
1
end
3
end

!!! Script runtime error !!!
  File "<stdin>",line 5, in waiting_for_hoge
                                        waiting_for_hoge(hoge=["3"])],
TypeError:'NoneType' object is not subscriptable
>>>
```
例外objectが返ってくるか，そのままraiseされてくるかの違い．