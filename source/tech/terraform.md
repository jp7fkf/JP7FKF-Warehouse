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
