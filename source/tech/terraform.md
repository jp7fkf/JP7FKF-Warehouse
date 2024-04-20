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
