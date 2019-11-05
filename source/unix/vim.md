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

## 文字コード
- check vim env encoding
  - `:set enc?`
- check file encoding
  - `:set fenc?`
- set encoding and save
  - `:set fenc=<encoding>`
- vimrcにこれ書いておくと良さそう
  ```
  set encoding=utf-8
  set fileencodings=iso-2022-jp,euc-jp,sjis,utf-8
  set fileformats=unix,dos,mac
  ```


## vimrc one example
```
"===== regular =====
set nocompatible
set autoindent
set expandtab
set tabstop=4
set shiftwidth=4
set showmatch
set smartcase
set smartindent
set smarttab
set nowrapscan
set title
set hlsearch
set cursorline
set wildmenu
set encoding=utf-8
let $LANG = "en_US.utf-8"
set clipboard+=unnamed
set backspace=indent,eol,start

inoremap {} {}<LEFT>
inoremap [] []<LEFT>
inoremap () ()<LEFT>
inoremap "" ""<LEFT>
inoremap '' ''<LEFT>

nnoremap <Esc><Esc> :<C-u>set nohlsearch<Return>

" autocmd BufWritePre * :%s/\s\+$//e
autocmd BufNewFile,BufRead *.{yang} set filetype=yang

"===== gtags =====
map <C-g> :Gtags
map <C-i> :Gtags -f %<CR>
map <C-j> :GtagsCursor<CR>
map <C-n> :cn<CR>
map <C-p> :cp<CR>

"===== dein.vim =====
if &compatible
  set nocompatible
endif

augroup MyAutoCmd
  autocmd!
augroup END

syntax enable

set runtimepath+=~/.vim/dein/repos/github.com/Shougo/dein.vim

if dein#load_state(expand('~/.vim/dein'))
    call dein#begin(expand('~/.vim/dein'))

    let g:dein_dir = expand('~/.vim/dein')
    let s:toml = g:dein_dir . '/dein.toml'
    let s:lazy_toml = g:dein_dir . '/dein_lazy.toml'

    call dein#load_toml(s:toml, {'lazy': 0})
    call dein#load_toml(s:lazy_toml, {'lazy': 1})

    call dein#end()
    call dein#save_state()
endif

if dein#check_install()
    call dein#install()
endif

filetype plugin indent on
```