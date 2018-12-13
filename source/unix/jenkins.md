# Jenkins
## jenkins install battle (2018-10-26 16:21:51 YudaiHashimoto)
  1. `yum -y install java-1.8.0-openjdk`
  2. レポジトリ追加
    curl -o /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
  3. パッケージ信頼性のための公開鍵追加．
    rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
  4. yum info jenkins
  5. yum -y install jenkins
  6. http://<server-ip>:8080 にアクセス
  7. /var/lib/jenkins/secrets/initialAdminPassword に鍵があるので見て入れる．
  