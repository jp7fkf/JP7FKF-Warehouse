# RENAT
## RENAT install battle
```
yum -y install centos-release-scl-rh
yum -y install python27
python --version

yum install -y epel-release
yum install -y gettext gcc net-snmp net-snmp-devel net-snmp-utils czmq czmq-devel python27-tkinter xorg-x11-server-Xvfb ghostscript firefox-60.2.1-1.el6.centos.x86_64 httpd vim git
pip install numpy pyte PyYAML openpyxl Jinja2 pandas paramiko lxml requests pdfkit pyvmomi PyVirtualDisplay
# yum search python | grep -i devel
# yum install -y python-devel (when errors occored at next step)
pip install netsnmp-py==0.3 

groupadd techno -o -g 1000
useradd robot -g techno
passwd  robot
```

```
cd
mkdir work
cd work
git clone https://github.com/bachng2017/RENAT.git renat
```

## basic setup

-  $RENAT_PATH/config/device.yaml
```
device:
    hoge1:
        type: hoge
        description: hoge1
        ip: x.x.x.x
```

-  $RENAT_PATH/config/auth.yaml
```
auth:
    plain-text:
        default:
            user: <user-name>
            pass: <password>
```

-  $RENAT_PATH/config/template.yaml
```
access-template:
    ssh-host:
        access: ssh
        auth: public-key
        profile: default
        prompt: \$
        append:
        init: unalias -a
    juniper:
        access: telnet
        auth: plain-text
        profile: default
        prompt: "(#|>) "
        append: ' | no-more'
        init:
    cisco:
        access: ssh
        auth: plain-text
        profile: default
        prompt: "*(#|>) "
        append:
        init:
snmp-template:
       juniper:
            mib: ./mib-Juniper.json
            community: public
            poller: renat
       cisco:
            mib: ./mib-Cisco.json
            community: public
            poller: renat
```


## python3.6 にしてみた
  - 相変わらずdecoratorで怒られた
    - `python3.6 -m pip install decorator` で解決．

## 問題点
  - ssh proxy を利用する際に問題がある．
    - device に下記のように書くとerrorとなる．
    ```
        vthunder_proxy:
        access: ssh
        auth: plain-text
        profile: xxx
        proxy_cmd: "/usr/bin/ssh -l jp7fkf -p xxxxx -i ~/.ssh/id_rsa -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -W %h:%p x.x.x.x"
        prompt: ".*(#|>)"
        login_prompt: "Username:"
        password_prompt: "Password:"
        append:
        init:
    ```
    - 問題はVChannel.pyのssh connection  部分にある．
    ```
      ### SSH
      if _access == 'ssh':
          out = ""
          local_id = self._ssh.open_connection(_ip,alias=name,term_type='vt100',width=w,height=h,timeout=_timeout)
          # SSH with plaintext
          if _auth_type == 'plain-text':
              if _proxy_cmd:
                  user        = os.environ.get('USER')
                  home_folder = os.environ.get('HOME')
                  port = 22
                  _cmd = _proxy_cmd.replace('%h',_ip).replace('%p',str(port)).replace('%u',user).replace('~',home_folder)
                  out = self._ssh.login(_auth['user'],_auth['pass'],proxy_cmd=_cmd)
              else:
                  _cmd = None
                  out = self._ssh.login(_auth['user'],_auth['pass'],False)
    ```
    - proxy_cmd というargument はそもそもSSHLibraryのloginメソッドに存在しない(# 2018-12-14 現在)．
    - proxy を自分で実装する必要がある．
    - ワークアラウンド的対処法としてrenatを実行するホストでsshuttle等をバックグランド実行しておき，renatには通常通りsshを実行させるという手段を取ることができる．

  - port番号指定できない
    - SSHLibraryのself.current.config.portを指定してやればいいっぽい．