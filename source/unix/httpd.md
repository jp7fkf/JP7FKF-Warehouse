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

### nginxでリバプロ
- https://qiita.com/HeRo/items/7063b86b5e8a2efde0f4

- httpをhttpsにリダイレクト
```
server {
    listen 80;
    server_name example.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443;
    ssl on;
    # ...
}
```
- websocketいれるとき下記いれないとだめっぽい．
```
proxy_http_version  1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
proxy_set_header Host $http_host;
proxy_set_header X-Real-IP $remote_addr;
```

- オレオレSSL証明書
  - https://dogmap.jp/2011/05/10/nginx-ssl/

- basic認証をつける
  - https://qiita.com/kotarella1110/items/be76b17cdbe61ff7b5ca

- sni
  - https://qiita.com/sawanoboly/items/5fd06f4787853c756122

- SANs, 2way
  - 1つの証明書で複数のcommon nameに対応
  - http://memories.zal.jp/WP/blog/20180716_2979.html

## h2o
  - h2oをcentos7にいれる
    - https://github.com/tatsushid/h2o-rpm を参考にyumでいれる．
    ```
    [jp7fkf@localhost]$ cat /etc/yum.repos.d/bintray-tatsushid-h2o-rpm.repo
    #bintray-tatsushid-h2o-rpm - packages by tatsushid from Bintray
    [bintray-tatsushid-h2o-rpm]
    name=bintray-tatsushid-h2o-rpm
    #If your system is CentOS
    baseurl=https://dl.bintray.com/tatsushid/h2o-rpm/centos/$releasever/$basearch/
    #If your system is Fedora
    #baseurl=https://dl.bintray.com/tatsushid/h2o-rpm/fedora/$releasever/$basearch/
    gpgcheck=0
    repo_gpgcheck=0
    enabled=1
    ```
    をかいて
    - `yum install h2o` する．

  - configをかいてstart/enable
  ```
  [root@localhost]# vim /etc/h2o/h2o.conf
    # configを書く

  [root@localhost]# systemctl restart h2o
  [root@localhost]# systemctl status h2o
  ● h2o.service - H2O - the optimized HTTP/1, HTTP/2 server
     Loaded: loaded (/usr/lib/systemd/system/h2o.service; enabled; vendor preset: disabled)
     Active: active (running) since Mon 2019-03-04 06:06:09 UTC; 1s ago
    Process: 29122 ExecStop=/bin/kill -TERM ${MAINPID} (code=exited, status=1/FAILURE)
   Main PID: 29980 (perl)
     CGroup: /system.slice/h2o.service
             ├─29980 perl -x /usr/share/h2o/start_server --pid-file=/var/run/h2o/h2o.pid --log-file=/var/log/h2o/error.log --port=0.0.0.0:443 --port=...
             ├─29981 /usr/sbin/h2o -c /etc/h2o/h2o.conf
             ├─29982 /usr/sbin/h2o -c /etc/h2o/h2o.conf
             └─29986 perl -x /usr/share/h2o/annotate-backtrace-symbols

  Mar 04 06:06:09 instance-1 systemd[1]: Started H2O - the optimized HTTP/1, HTTP/2 server.
  Mar 04 06:06:09 instance-1 h2o[29980]: start_server (pid:29980) starting now...
  ```

  - 残念ながらchromeなどではhttpsじゃないとhttp2にならずにhttp1.1で処理される．


## Let's Encrypt

### cert-botをいれて証明書をもらう
```
# hostが自分で引けるかどうか
[jp7fkf@localhost]$ host jp7fkf.dev
jp7fkf.dev has address x.x.x.x
[jp7fkf@localhost]$ host www.jp7fkf.dev
www.jp7fkf.dev is an alias for jp7fkf.dev.
jp7fkf.dev has address x.x.x.x

[jp7fkf@localhost]$ sudo yum install certbot
# installする

# certbotを動かす．オプション指定をして一気にキメている．
# --agree-tosはtosにagreeするy/nプロンプトを全agree
# --webrootでwebrootモード(一方はstandaloneモード．これだと更新時にhttpdがstopとなる．)
# -w でwebroot(認証用ファイルを置くディレクトリ)を指定(ここでは/var/www/html)
# -d でdomain name指定(複数ある場合は-d <domain>で複数記述)
[jp7fkf@localhost]$ sudo certbot certonly --agree-tos --webroot -w /var/www/html/ -d jp7fkf.dev -d www.jp7fkf.dev
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator webroot, Installer None
Enter email address (used for urgent renewal and security notices) (Enter 'c' to
cancel): mail@jp7fkf.dev
Starting new HTTPS connection (1): acme-v02.api.letsencrypt.org

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing to share your email address with the Electronic Frontier
Foundation, a founding partner of the Let's Encrypt project and the non-profit
organization that develops Certbot? We'd like to send you email about our work
encrypting the web, EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: N
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for jp7fkf.dev
http-01 challenge for www.jp7fkf.dev
Using the webroot path /var/www/html for all unmatched domains.
Waiting for verification...
Cleaning up challenges
Resetting dropped connection: acme-v02.api.letsencrypt.org

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/jp7fkf.dev/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/jp7fkf.dev/privkey.pem
   Your cert will expire on 2019-06-02. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le

[jp7fkf@localhost]$ sudo certbot renew --dry-run
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/jp7fkf.dev.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Cert not due for renewal, but simulating renewal for dry run
Plugins selected: Authenticator webroot, Installer None
Starting new HTTPS connection (1): acme-staging-v02.api.letsencrypt.org
Renewing an existing certificate
Performing the following challenges:
http-01 challenge for jp7fkf.dev
http-01 challenge for www.jp7fkf.dev
Waiting for verification...
Cleaning up challenges
Resetting dropped connection: acme-staging-v02.api.letsencrypt.org

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
new certificate deployed without reload, fullchain is
/etc/letsencrypt/live/jp7fkf.dev/fullchain.pem
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates below have not been saved.)

Congratulations, all renewals succeeded. The following certs have been renewed:
  /etc/letsencrypt/live/jp7fkf.dev/fullchain.pem (success)
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates above have not been saved.)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

IMPORTANT NOTES:
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.

[jp7fkf@localhost]$ sudo ls -l /etc/letsencrypt/live/
total 4
drwxr-xr-x. 2 root root  93 Mar  4 05:09 jp7fkf.dev
-rw-r--r--. 1 root root 740 Mar  4 05:09 README
[jp7fkf@localhost]$ sudo ls -l /etc/letsencrypt/live/jp7fkf.dev
total 4
lrwxrwxrwx. 1 root root  34 Mar  4 05:09 cert.pem -> ../../archive/jp7fkf.dev/cert1.pem
lrwxrwxrwx. 1 root root  35 Mar  4 05:09 chain.pem -> ../../archive/jp7fkf.dev/chain1.pem
lrwxrwxrwx. 1 root root  39 Mar  4 05:09 fullchain.pem -> ../../archive/jp7fkf.dev/fullchain1.pem
lrwxrwxrwx. 1 root root  37 Mar  4 05:09 privkey.pem -> ../../archive/jp7fkf.dev/privkey1.pem
-rw-r--r--. 1 root root 692 Mar  4 05:09 README

[root@localhost]# vim /etc/h2o/h2o.conf
  # # h2o.confに証明書情報を書き加える．
  # # Example
  # user: nobody
  # hosts:
  #   "localhost:443":
  #     listen:
  #       port: 443
  #       host: 0.0.0.0
  #       ssl:
  #         certificate-file: "/etc/letsencrypt/live/jp7fkf.dev/fullchain.pem"
  #         key-file: "/etc/letsencrypt/live/jp7fkf.dev/privkey.pem"

[root@localhost]# systemctl restart h2o
```

### renewするとき
```
[jp7fkf@localhost]$ sudo certbot renew
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/jp7fkf.dev.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Cert not yet due for renewal

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

The following certs are not due for renewal yet:
  /etc/letsencrypt/live/jp7fkf.dev/fullchain.pem expires on 2019-06-02 (skipped)
No renewals were attempted.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

### renewのdry-runもできる
```
[jp7fkf@localhost]$ sudo certbot renew --dry-run
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/jp7fkf.dev.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Cert not due for renewal, but simulating renewal for dry run
Plugins selected: Authenticator webroot, Installer None
Starting new HTTPS connection (1): acme-staging-v02.api.letsencrypt.org
Renewing an existing certificate
Performing the following challenges:
http-01 challenge for jp7fkf.dev
http-01 challenge for www.jp7fkf.dev
Waiting for verification...
Cleaning up challenges
Resetting dropped connection: acme-staging-v02.api.letsencrypt.org

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
new certificate deployed without reload, fullchain is
/etc/letsencrypt/live/jp7fkf.dev/fullchain.pem
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates below have not been saved.)

Congratulations, all renewals succeeded. The following certs have been renewed:
  /etc/letsencrypt/live/jp7fkf.dev/fullchain.pem (success)
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates above have not been saved.)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

### 強制更新 --force-renew
```
[jp7fkf@localhost]$ sudo certbot renew --force-renew --post-hook "systemctl restart h2o"
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/jp7fkf.dev.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Plugins selected: Authenticator webroot, Installer None
Starting new HTTPS connection (1): acme-v02.api.letsencrypt.org
Renewing an existing certificate
Resetting dropped connection: acme-v02.api.letsencrypt.org

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
new certificate deployed without reload, fullchain is
/etc/letsencrypt/live/jp7fkf.dev/fullchain.pem
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Congratulations, all renewals succeeded. The following certs have been renewed:
  /etc/letsencrypt/live/jp7fkf.dev/fullchain.pem (success)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Running post-hook command: systemctl restart h2o
```
  - ちなみに `certbot delete --cert-name <domain>` で取得した証明書を削除できる．

### 自動更新
  - cronを使う方法
  ```
  # crontab -l
  0 0,12 * * * /bin/certbot renew --post-hook "systemctl restart h2o"
  ```

  - systemdを使う方法
    - /etc/systemd/system/に下記をおく
    ```
    #certbot.service

    [Unit]
    Description=Certbot renew service
    After=h2o.target

    [Service]
    Type=oneshot
    ExecStart=/usr/bin/certbot renew --post-hook "systemctl restart h2o"

    [Install]
    WantedBy=default.target
    certbot.timer

    [Unit]
    Description=Renew Certification every day

    [Timer]
    OnBootSec=1min
    OnCalendar=*-*-* 04:00:00
    Unit=certbot.service

    [Install]
    WantedBy=timers.target
    ```

    - systemd darmon-reload
      - `sudo systemctl daemon-reload`
    - timer一覧
      - `sudo systemctl list-timers`

    - `journalctl -x -u certbot` とかでログをみてみるとか

## Let's EncryptなError
```
Attempting to renew cert (jp7fkf.dev) from /etc/letsencrypt/renewal/jp7fkf.dev.conf produced an unexpected error: Missing command line flag or config entry for this setting:
Input the webroot for jp7fkf.dev:. Skipping.
All renewal attempts failed. The following certs could not be renewed:
```
的な感じに言われる．
-  /etc/letsencrypt/renewal/jp7fkf.dev.conf に下記の記載がなかったりする．
```
  [[webroot_map]]
jp7fkf.dev = /path/to/document/root
www.jp7fkf.dev = /path/to/document/root
```
これがいつのまにか消えてたりするらしいので注意．

## SSL Server Testとか
- https://blog.yuu26.com/entry/20180618/1529333962

## apache2 (ubuntu18?)
### apache2でdocument rootをredirectする．
- たとえば `http:x.x.x.x/` を `http:x.x.x.x/git/` にリダイレクトしたければ，virtual hostなどに下記を書く．
- `RedirectMatch ^/$ /git/` 

### `/etc/apache2/*-enabled` と `/etc/apache2/*-avaialable`
- Q. なぜフォルダが2つあるのか(available, enabled)
  - A. 利用する/しないを分けるため．
- availableには利用しないがファイル自体はdumpできる．
- そうすると下記のようなコマンドで即座にenable可能．
- これらのコマンドは`/etc/apache2/*-enabled`に`/etc/apache2/*-available`へのシンボリックリンクを生成する．
```
# /etc/apache2/sites-avalable における適用と削除
a2ensite <filename>
a2dissite　<filename>

# /etc/apache2/mods-avalable における適用と削除
a2enmod <filename>
a2dismod <filename>

# /etc/apache2/conf-avalable における適用と削除
a2enconf <filename>
a2disconf <filename>
```