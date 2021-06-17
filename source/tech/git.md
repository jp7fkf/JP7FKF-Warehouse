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
## remote repositoryの用途
- `master`: 開発用．issueや開発
- `production`: プロダクション．商用と同じもの．
- `issue-<issue_no>`: issue_noのissueに紐づく修正
  - 新規機能開発も開発内容をissueとして登録し，それに準じて行う．

## production展開までの流れ
1. issue登録
2. issueにともなうコード変更とcommit
3. 該当のissue番号のissueに伴う変更を，issue-<issue_num>ブランチへpush
4. issueのコードレビューをissue ticketで実施．適宜squashしてcommitを綺麗にまとめる．
5. レビュー後，merge readyとなった段階で，masterブランチに対する，pull requestを発出．
6. 再度レビューを実施し，問題が認められなければmasterへmergeする．merge前にtestを実施しても良いが必須ではない．
7. ある時点のmasterのcommitに対してproduction展開のGO/NOGO判断を実施する．
コード中のversion番号はproductionリリースさせるversion番号に変更されていること．
8. production展開したいmasterのcommitについて，productionに対してpull requestを発出する．
  - このpull requestのtitleは下記のformatとする
  - `[prod] <rev_no>`
  - ref: `<rev_no>`: `v%d.%d.%d`のフォーマットで示されるバージョン(リビジョン)番号．
  - ex: `[prod] v1.2.3`
9. 必要に応じてこの時点でtestを実施する．testを自動化するのであればこのタイミング．
10. 最終的なGO/NOGOを判断する．GO判断とされた場合，そのpull requestをproductionへmergeする．
同時に，productionにmergeされたcommitに対してtagを打つ．これがリリースバージョンを指す．
また，同時に商用環境に展開する．

## issueの管理
- issueをあげたら，それに対するcommit メッセージは下記のformatとする
  - `[issue #<issue_no>] <messages>`
  - ex: `git commit -m '[issue #1] fix hogehoge'`
- issueに対応するリモートブランチを作成する場合，そのブランチ名は下記のformatとする．
  - `issue-<issue_no>`
  - ex: `git checkout -b issue-1`, `git push origin issue-1`
- ref:
    - `<issue_no>`: issue番号
    - `<messages>`: 任意のmessage

## その他
- productionにmergeする際は基本的にpull requestで実施する．
  - productionへのmergeタイミングをリリースタイミングとする．
    - リリースタイミングでは，リリースバージョンをtagとして打つ．
    - tagのformatは `v%d.%d.%d` とする．適宜インクリメントし，戻ることはなく，重複しない．

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

## 過去のコミットを消す
- rebaseしてreflog削除する
  - `git rebase -i hogehoge`
  - `git reflog expire --expire=now --all`

## Git ファイルの履歴を完全に削除する
- 秘密鍵など誤ってコミットしてしまった場合に履歴を完全に削除する手順
- 参考:[6.4 Git のさまざまなツール - 歴史の書き換え](http://git-scm.com/book/ja/ch6-4.html)

1. 動作確認用にブランチを作成して試す
`$ git checkout -b clean-key-file`

1. 動作確認用にブランチでgit filter-branchを実行
```
  $ git filter-branch --tree-filter 'rm -f common/key/id_rsa' HEAD
  Rewrite 856f0bf61e41a27326cdae8f09fe708d679f596f (12/12)
  Ref 'refs/heads/clean-key-file' was rewritten
```
clean-key-fileブランチでid_rsaが履歴から完全に削除されていることを確認する。

1. 全てのブランチを対象にgit filter-branchを実行
```
$ git filter-branch --tree-filter 'rm -f common/key/id_rsa' HEAD --all
```

1. reflogを削除
```
$ git reflog expire --expire=now --all
```

1. git gcを実行
```
$ git gc --aggressive --prune=now
```

1. git push --forceを実行
```
$ git push --force origin master
```
※ remote:error: denying non-fast-forward refs/heads/master ... とエラーが出た場合は
   下記のようにreceive.denynonfastforwardsをfalseにする。
```
$ git config receive.denynonfastforwards false
```

- ref: [Git ファイルの履歴を完全に削除する · GitHub](https://gist.github.com/ktx2207/3167fa69531bdd6b44f1#file-git_-md)
- ref: [機密データをリポジトリから削除する - GitHub ヘルプ](https://help.github.com/ja/github/authenticating-to-github/removing-sensitive-data-from-a-repository)

## git のコミット履歴をすべて消す
```
git checkout --orphan tmp # orphanオプションは孤立branchとして作成(branch派生をしない)
git commit -m "Initial Commit"
git checkout -B master # Bオプションは上書き．masterをtmpに上書きする
git branch -d tmp
```
- Reference
  - [git のコミット履歴をすべて消す（現時点の状態の1コミットだけにする） - Qiita](https://qiita.com/okashoi/items/6b1a8ca9a4b001200167)

## `git merge -X <ours/theirs> <branch>`
- conflictが発生した場合にour/theirsを強制的に採用する

## `git merge -s <ours/theirs> <branch>`
- theirs/oursのcommitを採用したmerge commitのみを作成する
  - 実態は変更されない
  - merge"した体にする"ときに使える
- ref
  - [Git - 高度なマージ手法](https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%81%95%E3%81%BE%E3%81%96%E3%81%BE%E3%81%AA%E3%83%84%E3%83%BC%E3%83%AB-%E9%AB%98%E5%BA%A6%E3%81%AA%E3%83%9E%E3%83%BC%E3%82%B8%E6%89%8B%E6%B3%95#r_manual_remerge)
```
MERGE STRATEGIES
       The merge mechanism (git merge and git pull commands) allows the backend merge strategies
       to be chosen with -s option. Some strategies can also take their own options, which can be
       passed by giving -X<option> arguments to git merge and/or git pull.
```