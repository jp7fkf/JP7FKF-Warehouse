# git

## gitlab install battle on ubuntu 18.04
- [How To Install and Configure GitLab on Ubuntu 18.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-gitlab-on-ubuntu-18-04)
- [Ubuntu18.04 +GitLab CE(Community Edition)を爆速構築 - Qiita](https://qiita.com/win-chanma/items/37680a4384be77f2a9e2)
- [Ubuntu18.04.1にGitlab11.2.3インストール＆日本語設定 - Qiita](https://qiita.com/hiko_t/items/263d6429b7fa9d2a001f)

### recommended machine specs
- 2 cores
- 8GB of RAM

### procedure
- `sudo apt update`
- `sudo apt install ca-certificates curl openssh-server postfix`
  - postfix はmail使わなければいらない気もするが，dependencyがあってinstallコケると面倒だからそのままやった
- `cd /tmp`
- `curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh`

- `less /tmp/script.deb.sh`
  - check script

- `sudo bash /tmp/script.deb.sh`
- `sudo apt install gitlab-ce`

- `sudo ufw status`
```
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
80/tcp                     ALLOW       Anywhere                  
443/tcp                    ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
80/tcp (v6)                ALLOW       Anywhere (v6)             
443/tcp (v6)               ALLOW       Anywhere (v6)
The above output indicates that the GitLab web interface will be accessible once we configure the application.
```
- `sudo vim /etc/gitlab/gitlab.rb`

```
/etc/gitlab/gitlab.rb
##! For more details on configuring external_url see:
##! https://docs.gitlab.com/omnibus/settings/configuration.html#configuring-the-external-url-for-gitlab
external_url 'https://example.com'
letsencrypt['contact_emails'] = ['sammy@example.com']
```
- `sudo gitlab-ctl reconfigure`

Username: root
Password: [the password you set]

/etc/gitlab/gitlab.rb
```
letsencrypt['auto_renew_hour'] = "12"
letsencrypt['auto_renew_minute'] = "30"
letsencrypt['auto_renew_day_of_month'] = "*/7"
letsencrypt['auto_renew'] = false
```