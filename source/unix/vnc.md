# VNC

## install
  - `yum install tigervnc-server`
  - `cp /lib/systemd/system/vncserver@.service /lib/systemd/system/vncserver@:3.service`
    - 何これという感じだが，"vncserver@.service"ファイルをvncserver@:(ディスプレイ番号).service"としてcopy.@の後ろはディスプレイ番号．1は利用されている可能性もあるため，今回は3とする．
  - 'vi /lib/systemd/system/vncserver@:3.service'
    - `<USER>`の部分をVNCでログインするユーザー名に変更．
  -  `vncpasswd` コマンドでVNCでのログインユーザにpasswordをつける．
  - `systemctl start vncserver@:3` でVNC Server を起動．
  - あとはつなぐだけ．