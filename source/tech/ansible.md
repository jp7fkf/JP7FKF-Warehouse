# Ansible

## 実行
- ex.) `ansible-playbook -i hosts playbook.yml -u ubuntu --private-key="hoge_rsa"`
  - `-i <inventory_file>`: インベントリファイルを指定．(default: `/etc/ansible/hosts`)
  - `-u <user_name>`: たぶんsshとかのusernameを指定．
  - `--private-key="hoge"`: たぶんssh接続時に鍵認証する場合の鍵を指定．
  - `-l`: 実行するグループを指定(hostsファイルの中のグループとかを指定するときに便利)

## シェルの標準出力を出す
- [ansible | シェルの標準出力を ansible-playbook コマンドで出力させる - Qiita](https://qiita.com/YumaInaura/items/5bee47677ec903551bb3)
```
- hosts: example
tasks:
  - name: execute shell
    shell: echo some message
    register: return_from_shell # 実行結果をansible変数に入れる
    changed_when: no
  - name: show previous shell stdout
    debug:
      msg: "{{ return_from_shell.stdout }}" # .stdout に標準出力の結果が入っている
```

## `ansible.cfg` の書き方tips
- 探し方
  - current dir.のansible.cfg
  - ENVのANSIBLE_CONFIG or ~/ansible.cfg
  - /etc/ansible/ansible.cfg

- セクション
  - [defaults]
  - [ssh_connection]

- [defaults]
```
[defaults]
# ssh_connection でいうStrictHostKeyCheckingと同等．ただしすでに~/ssh/known_hostsにあるホストとのcompare matchはするので注意．
host_key_checking = False
```

- [ssh_connection]
  これはsshの引数としてそのまま与えられると思えばよい．
```
[ssh_connection]

            #実行ディレクトリの<ssh_config>を読む．(defaultだと~/.ssh/configを読むのではなかろうか．たぶん．)
ssh_args =  -F ssh_config
            # ~/.ssh/known_hosts に存在しないときに確認なしで続行(yes扱い)．
            -o StrictHostKeyChecking=no
            # compare matchするknown_hostsファイルを指定しない．
            -o UserKnownHostsFile=/dev/null
```

- 参考
```
[defaults]
host_key_checking = False
interpreter_python=auto_silent
ask_vault_pass = True

[ssh_connection]
pipeline = false
ssh_args = -o ControlMaster=auto -o ControlPersist=1800s
```

## inventory ファイルの書き方tips
### general
- インベントリファイルと同じ階層の `group_vars` ディレクトリを配置し各グループのvars fileを配置する．インベントリファイルのgroupごとにファイルを分割し各ファイルにvarsを記載する　
  - [インベントリの分割 — Ansible ワークブック  ドキュメント](https://ansible-workbook.readthedocs.io/ja/latest/hensu2/inventory-no-bunkatsu.html)
  - `ansible-inventory` コマンドで各インベントリの変数状態をチェックできる．(ex. `ansible-inventory -i hosts.yml --list --yaml`, `ansible-inventory -i hosts.yml --graph`)
### init形式
- ssh 接続するときの情報を指定する．
```
[all:vars]
ansible_ssh_port=22
ansible_ssh_user=ansible
ansible_ssh_pass=p@ssW0rd
ansible_sudo_pass=p@ssW0rd
```

### yaml形式
```
web:
  hosts:
    server01:
      ansible_host: 10.0.0.1
asia:
  hosts:
    server01:
```

## ansible-playbookの書き方tips

## htpasswdを平文から生成
- aptとかで実行先のhostで`python3-passlib`をいれておく
- htpasswdパッケージでhash化できる．
```
- name: Configure web-gui accress users
  htpasswd:
    path: "{{app_dir}}/.htpasswd"
    name: "{{ item.name }}"
    password: "{{ item.password }}"
    owner: root
    group: root
    mode: 0640
  with_items:
    - "{{ users }}"
  no_log: True
```
- [htpasswd - manage user files for basic authentication — Ansible Documentation](https://docs.ansible.com/ansible/2.4/htpasswd_module.html)

## ログにcredentialを出さない
- なにもしないでansible-vaultされた平文credentialを扱うときとかにどうしてもログにでてきてしまいがち．
- task内で `no_log: True` を記述するとvarsとかのログが出なくなる．
- [Ansible 出力のロギング — Ansible Documentation](https://docs.ansible.com/ansible/2.9_ja/reference_appendices/logging.html)

### ファイルを削除する．
- ref): [[Ansible] file モジュールの基本的な使い方（ファイルやディレクトリの操作） - てくなべ (tekunabe)](https://tekunabe.hatenablog.jp/entry/2019/03/03/ansible_file_intro)

```
tasks:
- file:
    path: /etc/foo.conf
    state: absent
```

### shとかを実行
```
tasks:
- name: execute hoge.sh
  command: "/root/script/hoge.sh"
```

### ansible-vaultを使っているときにいちいちvalut passのaskやらargを書くのがしんどい
- ansible.cfgに下記を書く
  - ansible全般に言えることだけど実行するときにargsが決まっているならcfgに書いてしまってコマンドラインをreduceするのがスマート
```
ask_vault_pass = True
```

## role
- 操作コンポーネントのの単位などでひとまとまりにしたりできる．mainのplaybookでroleを指定してやるとそのroleのplaybookの内容が実行される．
- 基本的に `tasks/main.yml` に書かれた内容が実行され，それに付随するリソースとかが各ディレクトリにあると思えばいい．

###  ディレクトリ構成
```
tasks
handlers
defaults
vars
files
templates
meta
```

## handler
- notifyが記載されたtaskでchangedのイベントが起こったときにhandlerが最後に1回だけ呼び出される．
- handlerを定義(ex. systemctl restart)し，それを変化があったときに実行したいtask(ex. configの編集)でnotifyするイメージ．
- 書き方はtaskと同じ感じで`handlers`で書く
```
handlers:
  - name: restart app01
    systemd:
      name: app01.service
      state: restarted
    listen: "restart app01" (defaultでnameが利用されるの．name指定でnorifyできるのでnot required．nameとは異なる名前でnotifyしたいとき用．)

tasks:
  - name: hoge
    xxx: xxx
    notify: restart app01
```


## install battle
- CentOS7
```
[root@master1 ~]# yum -y install epel-release
[root@master1 ~]# yum -y install ansible
[root@master1 ~]# yum -y install openssh
```