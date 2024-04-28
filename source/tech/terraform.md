# Terraform

## きほんのき
- `terraform init`
  - `-reconfigure`
- `terraform workspace`
  - `list`
- `terraform plan`
- `terraform apply`
- `terraform import <resource.resource.name> <identifier>`
- `terraform state show <resource.resource.name>`

## tips
- 共有環境の場合，tf stateはlocalにおくのではなく，S3 bucket等に保存する．こうすることでlockの共有やstateの共有ができる．事故が防げる．

## un-terraform-managedな状態からterafform-managedにする
- 基本的には`.tf`を書き，`terraform import`で`.tf`内のリソースと紐付け，`terraform plan`してdiffを減らしていく作業になる．
- リソースと紐付けたら，`terraform state show`等をうまく使うとリソースの記述がサクサクできるので便利．

- References
  - [既存のAWS環境を後からTerraformでコード化する | DevelopersIO](https://dev.classmethod.jp/articles/aws-with-terraform/)

## terraformのversion変更にはtfenvが便利
- homebrewからinstallできる
  - `brew install tfenv`
  - https://github.com/tfutils/tfenv
```
% tfenv --help
Usage: tfenv <command> [<options>]

Commands:
   install       Install a specific version of Terraform
   use           Switch a version to use
   uninstall     Uninstall a specific version of Terraform
   list          List all installed versions
   list-remote   List all installable versions
   version-name  Print current version
   init          Update environment to use tfenv correctly.
```
- `tfnev local x.x.x` はないのか？
  - ない．かわりに `.terraform-version` で指定させることができる．
```
$ cat .terraform-version
0.6.16
```
こんなやつをterraformリソースのrootあたりにいれておけばいい．近い階層のやつの`.terraform-version`に記載されたversionが採用される．

## version系をかためる
- terraform versionは `required_version` でかためれる．
- 各種providerも `version` として記述してかためれる．
```
provider "google" {
  project = "myproject"
  region  = var.region
}

terraform {
  required_version = "1.0.0"
  backend "gcs" {
    bucket = "my-tfstate-bucket"
  }

  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "3.73.0"
    }
  }
}
```

## play/apply のtargetを指定して範囲を限定する
- 全部さらうのが理想的だが時間の関係などで非効率的な場合，変更範囲が掌握されていて限定的な場合に有効．
```
terraform apply --target=resource01 --target=resource02
```

## parallelismの値で並列数をあげて高速化とか
- apply, planで `--parallelism=30` みたいにする．defaultで10．
  - `export TF_CLI_ARGS_plan="--parallelism=30"` とかでも指定できるのでdefault値として変更したければshellのrcとかに書いておくもの良い．

## [terraform-provider-esxi を使って自宅 ESXi サーバに VM を立てよう - Qiita](https://qiita.com/entertvl/items/16789d7cb330450c4f27)

## [Terraform職人入門: 日々の運用で学んだ知見を淡々とまとめる - Qiita](https://qiita.com/minamijoyo/items/1f57c62bed781ab8f4d7)

## [Pluralith · GitHub](https://github.com/Pluralith)
- terraformから構成図をgenearteできるくん

## provider cache
- [TerraformのProvider Plugin Cacheを試す - CLOVER🍀](https://kazuhira-r.hatenablog.com/entry/2024/02/11/192735)
- [CLI Configuration | Terraform | HashiCorp Developer](https://developer.hashicorp.com/terraform/cli/v1.7.x/config/config-file#provider-plugin-cache)

## terraform bundle と terraform providers mirror
- [Terraform v1.0.0 で開発が終了した terraform-bundle の代替手段 #Terraform - Qiita](https://qiita.com/ryysud/items/e6e0d1701dc27ceec171)

## tfstateの移行
- aws s3からgoogle cloud storageにうつしたいとか，bucketを変更したいとかもこれと同様にlocalに一旦持ってきてからbackendを切り替えることで実施できる．
```
## GCSからLocalにもってくる
# backupをとる(optional)
$ terraform state pull >> terraform.tfstate.backup

# ======
# terraform {
# #  backend "gcs" {
# #    bucket = "dev-tfstate"
# #  }
# ...
# }
# ======

# bucketセクションをコメントアウトしてplanを実行すると怒られる．initが必要．
$ terraform plan
 ╷
 │ Error: Backend initialization required, please run "terraform init"
 │
 │ Reason: Unsetting the previously set backend "gcs"
 │
 │ The "backend" is the interface that Terraform uses to store state,
 │ perform operations, etc. If this message is showing up, it means that the
 │ Terraform configuration you're using is using a custom configuration for
 │ the Terraform backend.
 │
 │ Changes to backend configurations require reinitialization. This allows
 │ Terraform to set up the new configuration, copy existing state, etc. Please run
 │ "terraform init" with either the "-reconfigure" or "-migrate-state" flags to
 │ use the current configuration.
 │
 │ If the change reason above is incorrect, please verify your configuration
 │ hasn't changed and try again. At this point, no changes to your existing
 │ configuration or state have been made.

# migrateするためには -migrate-stateオプションが必要と言われる
# -reconfigureでもよいが，既存状態のことを考えずに今が正義になるので注意．強め．
$ terraform init
 Initializing the backend...
 ╷
 │ Error: Backend configuration changed
 │
 │ A change in the backend configuration has been detected, which may require migrating existing
 │ state.
 │
 │ If you wish to attempt automatic migration of the state, use "terraform init -migrate-state".
 │ If you wish to store the current configuration with no changes to the state, use "terraform
 │ init -reconfigure".
 ╵

# -migrate-state オプションをつけると，gcsからlocalにstate backendを変更する旨の確認が出る．yesでlocalにstateが来る．
$ terraform init -migrate-state

 Initializing the backend...
 Terraform has detected you're unconfiguring your previously set "gcs" backend.
 Do you want to copy existing state to the new backend?
   Pre-existing state was found while migrating the previous "gcs" backend to the
   newly configured "local" backend. No existing state was found in the newly
   configured "local" backend. Do you want to copy this state to the new "local"
   backend? Enter "yes" to copy and "no" to start with an empty state.

   Enter a value: yes

 Successfully unset the backend "gcs". Terraform will now operate locally.

 Initializing provider plugins...
 - Reusing previous version of jp7fkf.dev/dev/dev-terraform-provider from the dependency lock file
 - Using previously-installed jp7fkf.dev/dev/dev-terraform-provider v0.0.1

 Terraform has been successfully initialized!

 You may now begin working with Terraform. Try running "terraform plan" to see
 any changes that are required for your infrastructure. All Terraform commands
 should now work.

 If you ever set or change modules or backend configuration for Terraform,
 rerun this command to reinitialize your working directory. If you forget, other
 commands will detect it and remind you to do so if necessary.

# これでlocalにstate変更が完了


## LocalからGCSにもっていく
# backupとかは省略．概ね逆手順で実行できる．

# backendを記載する．
# ======
# terraform {
#   backend "gcs" {
#     bucket = "dev-tfstate"
#   }
# ...
# }
# ======

# 同様に-migrate-steteを付与してinitをすると，localからgcsにbackendを変更する旨の確認が出る．yesでマイグレ．
$ terraform init -migrate-state
 Initializing the backend...
 Do you want to copy existing state to the new backend?
   Pre-existing state was found while migrating the previous "local" backend to the
   newly configured "gcs" backend. No existing state was found in the newly
   configured "gcs" backend. Do you want to copy this state to the new "gcs"
   backend? Enter "yes" to copy and "no" to start with an empty state.

   Enter a value: yes

 Successfully configured the backend "gcs"! Terraform will automatically
 use this backend unless the backend configuration changes.

 Initializing provider plugins...
 - Reusing previous version of jp7fkf.dev/dev/dev-terraform-provider from the dependency lock file
 - Using previously-installed jp7fkf.dev/dev/dev-terraform-provider v0.0.1

 Terraform has been successfully initialized!

 You may now begin working with Terraform. Try running "terraform plan" to see
 any changes that are required for your infrastructure. All Terraform commands
 should now work.

 If you ever set or change modules or backend configuration for Terraform,
 rerun this command to reinitialize your working directory. If you forget, other
 commands will detect it and remind you to do so if necessary.
```
- ref
  - [tfstateをローカルとS3間で移行してみた | DevelopersIO](https://dev.classmethod.jp/articles/tfstate-s3-local-migration-method/)

## meta arguments
- [Resources Overview - Configuration Language | Terraform | HashiCorp Developer](https://developer.hashicorp.com/terraform/language/resources)
- `depends_on`: 依存関係を明示指定する．基本的にterraformが自ずと解決するので，使いたくない．
- `count`: nコ作れる
- `for_each`: `count`と似ているがk/v map/setで別名をつけつつ作ったりできる．
- `provider`
- `lifecycle`
  - `create_before_destroy`: replaceするときとかに消す前に作る(他リソースとの依存関係がある場合など)
  - `prevent_destroy`: 消させない/destroyさせない
  - `ignore_changes`: remote changesをignoreする
  - `replace_triggered_by`: ここで指定した値が変わったらreplaceする

## [Terraform 1.5 で追加された import ブロックと HCL 自動生成オプションの組合せが便利](https://zenn.dev/cloud_ace/articles/usage-terraform-import-block)
- > Note: Import blocks are only available in Terraform v1.5.0 and later.
- import.tfを書く
  ```
  import {
    id = {id}
    to = {resource}+{resource_name}
  }
  ```
- `terraform plan -generate-config-out=generated.tf`
  - `generated.tf` が作られる．import相当のデータをもとにtfファイルができる．
  - int/stringの識別がうまくないと`jsonencode()`的に出力されることがある．適宜修正．
- `terraform plan`
  - importのplanをしてみる．importのみが実行予定となるはず．
- `terraform apply`
  - import実行．stateファイルにはこの段階で書かれる．
- `terraform plan`
  - no changesのはず．
