# virsh

## Commands
### Info.
  - help
    - `virsh help`

  - VM一覧
    - `virsh list` (稼働中のみ)
    - `virsh list --all` (すべて)

  - ネットワーク一覧
    - virsh net-list (稼働中のみ)
    - virsh net-list --all (すべて)

  - リソースの利用状況を見たい
    - `virt-top`

### Actions
  - VMを起動
    - `virsh start [name]`
  - VMを停止
    - `virsh stop [name]`
  - VMを強制停止
    - `virsh destroy [name]`
  - VMを一時停止
    - `virsh suspend [name]`
  - VMを再起動
    - `virsh reboot [name]`
  - コンソール接続
    `virsh console [name]`
      - `ctrl + ]` で抜ける

### VM Operations
  - 通常のインストール手順
    ```
    1. イメージの作成
    2. インストールの実行
    3. OSのインストール
    ```
    1. イメージの作成
      - `qemu-img create -f qcow2 [image_name] [size]`
        - `例) qemu-img create -f qcow2 example.img 20G`

    2. インストールの実行
      ```
      virt-install \
      --connect qemu:///system \
      --name=[ドメイン名] \
      --ram=[メモリ:単位はメガ] \
      --disk path=[作成したイメージファイルのパス] \
      --vcpus=[CPUの割り当て数] \
      --os-type=linux \
      --os-variant=rhel6 \
      --network bridge=br0 \
      --nographics \
      --location='http://ftp.riken.jp/Linux/centos/6/os/x86_64/' \
      --hvm \
      --accelerate \
      --extra-args='console=tty0 console=ttyS0,115200n8'
      ```

    3. OSのインストール

  - VMをコピーする
    ```
    virt-clone \
    --original [origin_name] \
    --name [new_name] \
    --file [new_path]
    ```

  - VMを削除
    - `virt undefine [guest_name]`

  - VMの設定変更
    - xmlを編集
      - `virsh edit [name]`
    - xmlの内容を適用
      - `virsh define [path_to_xml]`
      - 設定の流れとしてはedit->define．CPUやメモリの変更は要再起動。

  - 動的にネットワークインターフェースを追加
    - `virsh atatch-interface [name] [int_type] [int_src]`
    - `例) virsh attach-interface example bridge virbr1`

  - ネットワーク設定
    - 仮想ブリッジを作成したい
      - XMLを作成
      ```
      /etc/libvirt/qemu/networks/network-name.xml
      <network>
      <name>network-name</name>
      <bridge name="virbr1" stp='on' delay='0'/>
      </network>
      ```
      - 適用
        - `virsh net-create network-name.xml`
          - 作成したネットワークをVMで利用する時はVMのxmlに設定してインターフェースを追加するなり再起動するなりすればOK。

  - スナップショット関連
    - スナップショットを作成
      - `virsh snapshot-create-as [name] [snapshot_name] [descriptions]`
        `例) virsh snapshot-create-as examle examle-snap "desc"`
      - 作成中はVMが一時停止する

    - スナップショットを確認
      - `virsh snapshot-list [name]`

    - スナップショットから復元
      - `virsh snapshot-revert [name] [snapshot_name]`
      - VM稼働中に実行すると復元後リブートする．停止中に実施するのがよいらしい．

    - スナップショットの削除
      - `virsh snapshot-delete [name] [snapshot_name]`

    - 無停止でスナップショットを作成
      - `virsh snapshot-create-as [name] [snapshot_name] [descriptions] --disk-only --atomic`
      - under development

## Reference
  - ref): http://og732.hatenadiary.com/entry/2014/02/28/002709