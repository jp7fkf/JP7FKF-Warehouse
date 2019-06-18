# mac commands

## macOS command でzipパスワードをつける
zipcloak hoge.zip

## Go to the Folder in Finder
- `Commnad + Shift + G`
- unixで `cd` 打つ感じでパスを入れる感じ．
- `~/Library/hoge.txt` 的な

## MacでIPv6の有効/無効
  - sudo networksetup (-setv6off | -setv6automatic) <networkservice(ex.Wi-Fi)>

## iterm2-tips-network-engineers
  - http://movingpackets.net/2014/04/13/iterm2-tips-network-engineers/
  
## brew doctorで怒られる
- `brew doctor` すると
```
% brew doctor
Please note that these warnings are just used to help the Homebrew maintainers
with debugging if you file an issue. If everything you use Homebrew for is
working fine: please don't worry or file an issue; just ignore this. Thanks!

Warning: "config" scripts exist outside your system or Homebrew directories.
`./configure` scripts often look for *-config scripts to determine if
software packages are installed, and which additional flags to use when
compiling and linking.

Having additional scripts in your path can confuse software installed via
Homebrew if the config script overrides a system or Homebrew-provided
script of the same name. We found the following "config" scripts:
  /Users/jp7fkf/.pyenv/shims/python2-config
  /Users/jp7fkf/.pyenv/shims/python3.6m-config
  /Users/jp7fkf/.pyenv/shims/python2.7-config
  /Users/jp7fkf/.pyenv/shims/python-config
  /Users/jp7fkf/.pyenv/shims/python3-config
  /Users/jp7fkf/.pyenv/shims/python3.6-config
```
的な感じで怒られる．
- `*-config` があるとbrewのやつと衝突する危険があるよということでwarningが出ているよう．
  - pythonのconfigがこの体裁なんだな． `/Users/jp7fkf/.pyenv/shims` あたりがPATHに入っているので仕方なさそう．
- `alias brew="PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/sbin brew"` をすることでPATH指定して実行することで解決．
```
% brew doctor
Your system is ready to brew.
```