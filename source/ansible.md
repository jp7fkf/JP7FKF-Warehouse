## Ansible

### シェルの標準出力を出す
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