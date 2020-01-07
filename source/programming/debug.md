# debug

## メモリリーク
-  Valgrind
  - バイナリさえあれば実行可能．
  - `sudo apt install valgrind` とかでサクッとインストール．
    - 依存関係があるライブラリはあるかもしれないので，適宜うまくやる．
  - 後は実行
    ```
    $ g++ -g leak.cpp
    $ valgrind --leak-check=full ./a.out
    ```
  - `valgrind --leak-check=full <exefilename>` で実行できて簡単．

- ref: [メモリリークチェッカー - Qiita](https://qiita.com/bsdhack/items/a4a14de78213c8f7177c)
- ref: [C/C++でのメモリリーク検出方法 〜AddressSanitizer, Valgrind, mtrace〜 - kivantium活動日記](http://kivantium.hateblo.jp/entry/2018/07/14/233027)
