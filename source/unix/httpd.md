# httpd

## apache

## nginx

### nginxでhttps化のためにSSL証明書を設定する．
  - 鍵をつくる
    ```
      # 鍵生成
      sudo openssl genrsa -des3 -out ./ssl.hoge.com.key 2048
      # 鍵からcsrをつくる
      sudo openssl req -new -key ./ssl.hoge.com.key -out ./ssl.hoge.com.csr
      # みる
      less ssl.hoge.com.csr
    ```
    - これで鍵はできた．これを用いて証明書を発行する．

  - 発行された証明書をinstall
    ```
      # 証明書を結合する（必要があれば）
      cat SSLサーバ証明書ファイル(crt) 中間証明書ファイル(cer) > 新しい結合ファイル(pem)
      # 1. SSL証明書
      # 2. SSL中間CA証明書
      # 3. クロスルート証明書
      # の順番で結合されているべきらしい．

      # 権限をroot onlyに変更．所有者，グループをrootに変更
      chmod 400 SSLサーバ証明書の保存ディレクトリ/結合後のファイル名
      chown root:root SSLサーバ証明書の保存ディレクトリ/結合後のファイル名

      # nginx の証明書参照先を確認/変更．
      vi /etc/nginx/conf.d/ssl.conf
      ===
      ssl_protocols <use_protocol>
      server {
        listen <port_num>;
        ssl <ena/disable>;
        server_name <server_fqdn(common_name)>;
        ssl_certificate <path_to_ssl_pem>;
        ssl_certificate_key <path_to_ssl_key>;
      ...
      }
      ===

      # config validation and reload
      service nginx configtest && service nginx reload
      # (same as [/usr/sbin/]nginx -t && [usr/sbin/]nginx -s reload)
      # (same as service nginx configtest && service nginx reload)

    ```
    - SSL秘密鍵にpassphraseが設定されているとnginx起動時にpassphraseの入力が求められるらしい．
      - これを回避するには単純に秘密鍵ファイルからpassphraseを抜いてあげればよい
      - `openssl rsa -in cert.key -out cert.key` してpassphraseを入力してpassphraseを削除した秘密鍵ファイルにしてやる．
        - 念の為バックアップを取ったほうがいいと思う．