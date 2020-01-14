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
