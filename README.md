AWS Copilot CLI のお試し用

- frontend: nginx
- backend: Django

接続確認... `http://[deployed-ipaddress]/admin`

以下、メモ

# 用語整理

## Application

複数の Service や env をまとめた全体を指す。

"プロジェクト" に近い概念。

## Service

Application を構成する機能ごとの単位。

frontend(nginx) と backend(Django) みたいな。

## env

Application のデプロイ先であるインフラ環境、AWSリソース群。
test, prod 環境など用途に応じて環境を分離できる。
⇒VPC/Subnet やロードバランサー、ECS クラスター、などが分かれることになる。

## Pipeline

Git リポジトリへの push をトリガーに CI/CD を行えるパイプラインを定義できる。
事前に GitHub Personal Access Token を発行しておく。`repo`, `admin:repo_hook`にチェックを入れる

実体は CodePipeline と CodeBuild。

- `copilot/pipeline.yml`: CodePipeline の設定を定義（Copiliot の独自記法?）
- `copilot/buildspec.yml`: CodeBuild のジョブ定義

# How to use

```shell
$ git clone https://github.com/skokado/django-pilot
$ cd django-pilot/
```

## Applocation の初期化

copilot app init

## env 作成

copilot env init --name test --profile default --app [app-name]

- 作成モード
  - 専用のVPCを新規作成(CIDRは自動採番)
  - 専用のVPCを新規作成(CIDRを手動入力)
  - 既存VPCを利用(Public Subnet が作成されている必要がある)

## Service の登録

copilot svc init --name [svc-name]

※対話形式で色々聞かれる。参考: https://github.com/aws/copilot-cli/wiki/Service-Types

## Pipeline の登録

※既に`copilot/buildspec.yml`, `copilot/pipeline.yml`が存在する前提

copilot pipeline update

※CodePipeline が成功すると deploy まで自動実行される

## 起動

copilot svc deploy --name backend --env test --tag [image-tag]

## 停止

copilot svc delete --name backend --env test --yes

## 環境削除

copilot app delete --yes
