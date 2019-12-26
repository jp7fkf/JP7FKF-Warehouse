# sh
## shell scripts
- [Linuxの対話がめんどくさい?そんな時こそ自動化だ！-expect編- - Qiita](https://qiita.com/ine1127/items/cd6bc91174635016db9b)

- 実行したコマンドラインも表示する
  - `set -x` を書く．(`set +x` で非表示になる．)
  - `#!/bin/sh -x`， `#!/bin/bash -x` と頭にかいてもよい．
  - https://www.atmarkit.co.jp/ait/articles/1805/10/news023.html

## config ファイルとかを別で管理する
- shell script のconfigだけ別ファイルにしたいとき．
- setting.conf
```
TEST=hogehoge
```
- test.sh
```
#!/bin/sh

. ./setting.conf
echo $TEST
```

## 空文字列かどうかを調べる
- `-z` or `-n`
- ex.
```
if [ -n "$STR" ]; then
   echo "non zero length"
fi
if [ -z "$STR" ]; then
   echo "zero length"
fi
```

## コマンドの実行結果を変数に入れる
- back quartを使う or `$()` を使う．
- `` FILE=`find *` ``
- `FILE=$(find *)`

## set
- `e`, `x`, をよしなに`+`,`-`すると便利
- `set -x`
  - Print shell command before execute it. This feature help programmers to track their shell script.

- `set -e`
  - If the return code of one command is not 0 and the caller does not check it, the shell script will exit. This feature make shell script robust.
  - set -e and set -x often appear at the head of shell script:

- also we can do it as follows
  - `sh -xe shell_script.sh`
- Reference: http://julio.meroh.net/2010/01/set-e-and-set-x.html