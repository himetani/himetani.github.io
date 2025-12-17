+++
title = "Databricks Asset Bundlesの実運用に向けて"
date = 2025-12-14T14:38:38+09:00
draft  = false
tags = ["databricks", "terraform"]
+++

Databricks Asset Bundlesを実運用する上で役に立ちそうな機能や使い方について調べました。

<!--more-->

# Databricks Asset Bundlesとは

Databricks上の各種リソースをコードとして定義・管理・デプロイすることを可能にするツールです。
Bundleと呼ばれるYAML形式のファイルでリソースを定義し、Databricks CLIに組み込まれた`databricks bundle`サブコマンドでデプロイを実行できます。

```bash
# Bundle定義のバリデーション
databricks bundle validate

# Bundleのデプロイプランの表示
databricks bundle plan

# Bundleのデプロイ
databricks bundle deploy
```

(`databricks bundle deploy`)を使って、Bundleのデプロイを実行。します
その他にも、Bundle定義のバリデーション(`databricks bundle validate`)、デプロイプランの表示(`databricks bundle plan`)など、デプロイ管理に必要なコマンドが準備されています。
GUI上での手作業での設定するのではなく、宣言的なコードでの管理に移行することで再現性のある安全なデプロイを実現できます。

内部的にはTerraformが利用されていて、Databrticks Asset BundlesではYAMLで記述されたBundle定義からがTerraform

# 利用した環境

# Databricks CLIの認証

#
