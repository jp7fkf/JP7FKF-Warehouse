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