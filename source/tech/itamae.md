# itamae 

## install
```
% cat Gemfile
source "https://rubygems.org"

gem "itamae"
gem 'itamae-plugin-recipe-selinux'
```
- `bundle install --path vender/bundle`

## exec
- `bundle exec itamae ssh -h <ip_address> -p 22 -u root recipes/zab-pri-itamae.rb`

## tips
