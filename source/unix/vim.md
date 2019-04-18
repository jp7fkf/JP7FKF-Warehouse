# vim

## how to use vim
  - `hjkl`: cursor
  - `gg`: go to top
  - `G`: go to bottom
  - `/<pattern>`: inc search
    - `n`, `shift + n`: next/previous

## commands
  - `:w`: save
  - `:q`: quit
  - `:set paste`: vimにコピペするときにtabとかが崩れない．
  - `:%s/"置換前"/"置換後"/g`: ファイル内全置換(% means all of file)
    - ex. `:1,100s/"置換前"/"置換後"/g`: 1-100行目でマッチするもの全て．
  - `:%s/\(hoge\)/h(\1)/`: 正規表現hogeにmatchする文字列を置換後に流用(\1)

## comment out
  1. `ctrl + shift + v` and block selection
  2. `shift + i`: and input comment chars.
  3. `esc`

## 