# Unix Basic Commands

## df -h
  - 容量みれる

## du -sh *
  - 今の階層下のフォルダやファイルの容量をみる．

## traceroute
- 尻尾になんかついてきたら
  - !H: [Type=3/Code=1] Host Unreachable 
  - !X: [Type=3/Code=13] Communication Administratively Prohibited
  - !N: [Type=3/Code=0] Net Unreachable
  - !P: [Type=3/Code=2] Protocol Unreachable
  - !F: [Type=3/Code=4] Fragmentation Needed and DF set
  - !S: [Type=3/Code=5] Source route failed
  - !V: [Type=3/Code=14] Host Precedence Violation
  - !<icmp_code_num>

## tmux とかnohupするの忘れて長時間かかるコマンドをやってしまったとき
```
$ rpmbuild -ba hoge.spec // やっちまった
$ # Ctrl+Zで中断
$ bg 1
$ jobs 1
$ disown %1
```
- nohupとdisownの違いは
  - nohup:コマンドをハングアップシグナル無視で実行させる。
  - disown:nohupをつけ忘れた時、途中からバックグラウンドでの実行へ切り替える