# Ruby on Rails

## on heroku
  - [Rails3+herokuで開発はsqlite3、heroku上はPostgreSQLを使う設定](https://daipresents.com/2012/post-5016/)

## bundle install でHTTPError
```
Fetching source index from https://rubygems.org/
Retrying fetcher due to error (2/4): Bundler::HTTPError Could not fetch specs from https://rubygems.org/
Retrying fetcher due to error (3/4): Bundler::HTTPError Could not fetch specs from https://rubygems.org/^C
```
  - `gem update --system` で無事解決した．
  

## はじめての
```
$ rbenv -v
rbenv 1.1.1

$ rbenv install --list
$ rbenv install 2.5.0
$ rbenv global 2.5.0
$ rbenv rehash
$ ruby -v
$ bundle -v
$ mkdir ~/workspace
$ cd ~/workspace
$ rbenv local 2.5.0
$ bundle init
Gemfileを編集し# gem "rails"をコメント解除する
$ bundle install --path=vendor/bundle
$ bundle exec rails -v
$ bundle exec rails new test
$ rails server
```

## scaffoldで作成したファイルを全削除
```
rails generate scaffold "name" "field:type"〜
rails destroy scaffold "name"
rails generate scaffold
rails db:drop
rails db:create
rails db:migrate
```

## slimをぶちこむ
```
    gem 'slim-rails'
    bundle install
    gem install html2slim
    rbenv rehash
    for file in app/views/devise/**/*.erb; do erb2slim $file ${file%erb}slim && rm $file; done
```

## locale
  - i18n対応
    - `gem 'rails-i18n', '~> 5.1`
  - enum使うときにはこれがあると便利
    - `gem 'enum_help'`

  - application.rb
    ```
    # i18n
    config.i18n.load_path += Dir[Rails.root.join('config', 'locales', '**', '*.{rb,yml}').to_s]
    config.i18n.default_locale = :ja
    config.i18n.available_locales = [:ja, :en]
    ```
  - `config/locales/` 配下に各ロケールの表記を記述する．
  - `t('jaの階層を除いたパス')` 関数を利用するとlocalesファイルの内容にあわせて各ロケールに応じたtranslationが行われる．
    - localesファイルはlocales配下にあれば特にパス指定等は必要ない．どの階層にあっても読まれるようだ．

## 問答無用でdb作り直す
  ```
    rails db:drop
    rails db:create
    rails db:migrate
  ```

## 1つのformで複数のmodelを編集したい
  - 基本的にassociationを書いておいたほうがよさそう．
  - form_forの中でfields_forをネストさせるのが定石っぽい
  - http://tkot.hatenablog.com/entry/2013/07/06/010617
  - 関連がないときのベストプラクティスはこれ？(本当にこれがいいかは不明)
    ```
      def update
        if [@model1.valid?, @model2.valid?].all?
          ActiveRecord::Base.transaction do
            @model1.save
            @model2.save
          end
        else
          render :edit
        end
      end
    ```