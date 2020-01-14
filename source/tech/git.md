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

## git rm
- Gitリポジトリからファイルを削除するには3パターンある．
1. リポジトリから削除，かつ，ディレクトリから削除（i.e. 完全に削除）．
2. リポジトリから削除，かつ，ディレクトリには残す．
3. リポジトリには残す，かつ，ディレクトリから削除（あんまりないかも）．

```
$ git rm FILENAME           ## 1.の場合．
$ git rm --cached FILENAME  ## 2.の場合．
$ rm FILENAME               ## 3.の場合．
```

- 1.の場合については，以下と同じ．
```
$ rm FILENAME       ## ファイルが deleted になる．
$ git add FILENAME  ## deletedなことがステージされる．
```
ref: [Gitリポジトリからファイルを削除したい [QumaWiki]](https://www-he.scphys.kyoto-u.ac.jp/member/shotakaha/dokuwiki/doku.php?id=toolbox:git:rm:start)

## git diff
- `--name-only`: 変更があるファイル名のみを表示する

## commit をまとめる
### squash
- 作業用ブランチを切ってご自由にcommit する．
- push する前に，commitをひとまとめにするためにbranchを作成．そのブランチで該当のcommitをたくさんしたブランチを`git merge --squash <branch_name>`する．
- ひとまとめにするcommitをうつ．
- push!

### rebase
- `git rebase -i <commit_id>`
```
(p)pick コミットをそのまま残す．
(r)reword コミットメッセージを変更．
(e)edit コミット自体の内容を編集．
(s)squash 直前のpickを指定したコミットに統合．メッセージも統合．
(f)fixup  直前のpickを指定したコミットに統合．メッセージは破棄．
```
- リモートにpushするときに場合によって(local/remoteのbranchの状況によって)は怒られる．force push するしかない？
- ref:
```
# Rebase xxxxxx..xxxxxx onto xxxxxx (x commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

## 個人的な決まり
### issueの管理
- issueをあげたら，それに対するcommit メッセージは下記のformatとする
  - `[issue #<issue_no>] <messages>`
- issueに対応するリモートブランチを作成する場合，そのブランチ名は下記のformatとする．
  - `issue-<issue_no>`
- ref:
    - <issue_no>: issue番号
    - <messages>: 任意のmessage

### その他
- productionにmergeする際は基本的にpull requestで実施する．

## git tag
- tagをうつ
  - `git tag <tag_name>`
    - タグに関する情報(author, date, comment)は入らない．
  - `git tag -a "<tag_name>" -m "<comment>" <commit_hash>`
      - タグに関する情報(author, date, comment)が入る．
    - ex. `git tag -a "v0.1.1" -m "version 0.1.1" xxxxxxxx`
    - <commit_hash>を抜いて打つと現在のブランチの最新コミットに対して付与される．
- tag群を見る
  - `git tag`
  - `git tag --list`, `git tag -l`
- tag群を検索
  - `git tag -l <regexp>`
    - ex.: `git tag -l 'v1.2.*'`
- tagをpush して共有する
  - `git push origin <tag_name>`
- 詳細情報まで見たい
  - `git show <tag_name>`

## github
- commitメッセージとして`close #<issue_no>`があると，そのissueはcloseされる．
  - ほかには以下のキーワードの後に Issue 番号を入れると，その Issue がクローズされる．
    - close
    - closes
    - closed
    - fix
    - fixes
    - fixed
    - 解決
    - resolves
    - resolved
  - プルリクのメッセージでも同様の機能が使えて，プルリクの場合にはマージされたタイミングでcloseされる．
  - ref: [キーワードを使って Issue をクローズする - GitHub ヘルプ](https://help.github.com/ja/github/managing-your-work-on-github/closing-issues-using-keywords)
