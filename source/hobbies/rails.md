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
  