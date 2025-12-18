+++
title = "Databricks Asset Bundlesの運用を意識した設計"
date = 2025-12-14T14:38:38+09:00
draft  = true
tags = ["databricks", "terraform"]
+++

Databricks Asset Bundlesの運用を意識して、使うと良さそうな機能や設定方法について調べました。

<!--more-->

`Databricks Asset Bundles`は便利で実用的なツールであるにも関わらず公式ドキュメント以外にあまり情報がないように思います。
そこで、本番利用を見据えて機能や設定方法について調べ、まとめてみました。
この記事はDatabricks Advent CalendarのN日目の記事です。

# 調査に使った環境やツール
- Databricks CLI (v0.280.0)
- Databricks Free Edition

Databricks Free EditionはDatabricksが提供する無料で利用できる環境です。
AWS/Azure/GCPなどのクラウドベンダーのアカウントが不要でDatabricksにサインアップすれば利用できます。
ネットワーク関連などクラウドベンダ依存の設定をできない、computeはserverlessのみ、などいくつか制限がありますが、
今回試したかった`Databricks Asset Bundles`の基本的な機能はサポートされているのでこちらを使います。

# Databricks Asset Bundlesとは

Databricksのジョブやnotebookなど各種リソースをコードとして定義・管理・デプロイすることを可能にするツールです。
Bundleと呼ばれるYAML形式のファイルでリソースを定義し、Databricks CLIに組み込まれた`databricks bundle`サブコマンドでデプロイを実行できます。最小構成のBundle定義の例を示します。

```yaml {filename="databricks.yml" caption="最小構成のDatabricks Asset Bundle定義例"}
bundle:
  name: minimal_bundle

targets:
  dev:
    workspace:
      host: https://adb-xxxxxxxxxxxx.xx.azuredatabricks.net

resources:
  jobs:
    hello_job:
      name: hello-job
      tasks:
        - task_key: hello_task
          notebook_task:
            notebook_path: ./src/hello.py
```

Bundle定義を作成したのち、一連のコマンド操作を経て、Databricksにジョブをデプロイできます。


```bash
> databricks bundle validate # Bundle定義のバリデーション
Name: asset_bundles_example
Target: dev
Workspace:
  Host: https://xxxx.cloud.databricks.com
  User: my_user_name
  Path: /Workspace/Users/my_user_name/.bundle/asset_bundles_example/dev

Validation OK!

> databricks bundle plan # プランの作成
create jobs.hello_job

Plan: 1 to add, 0 to change, 0 to delete, 0 unchanged

> databricks bundle deploy # デプロイの実行
Uploading bundle files to /Workspace/Users/my_user_name/.bundle/asset_bundles_example/dev/files...
Deploying resources...
Updating deployment state...
Deployment complete!
```


{{< figure src="hello-job.png" alt="hello-jobが作成される" caption="hello-jobが作成される" >}}

これらのコマンドの内部では、`Terraform`が使われています。`YAML`で記述されたBundle定義が`JSON`形式のTerraform定義に変換され、`Terraform`が変更を適用しています。
デバッグログを出力してみると、`Databricks API`が利用されていることが確認できます。


作成されたジョブはDatabricks workspaceのGUIからだけでなく、CLIから起動させることができます。

```bash
> databricks bundle run hello_job
Run URL: https://xxxx.cloud.databricks.com/?o=2439733251406638#job/165710993591161/run/1034736749159003

2025-12-18 16:37:02 "hello-job" RUNNING
2025-12-18 16:37:41 "hello-job" TERMINATED SUCCESS
```

{{< figure src="hello-job-run.png" alt="CLIから起動されたhello-job" caption="CLIから起動されたhello-job" >}}

## 1. targetでデプロイ先の環境を切り替える
-

## 2. target毎に異なるResourceをデプロイする
- 

## 3. modeを使いtarget毎に共通のメタデータをつける
-

## 4. presetでカスタマイズした独自のmodeを作る
- 

## 5. 個人ユーザでなくService Principalを使う
- CI/CDに触れる
- Secretsに触れる

## 6. 肥大化したBundleは複数のBundleに分割する
- 

## 7. シェルスクリプトの代わりにscriptsを活用する
- 

## 8. `databricks bundle plan`で作られたPlanファイルをTerraformで可視化する
- 
