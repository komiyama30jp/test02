---
date: "2024-10-30"
lastmod: "2024-10-30"
---

# ECS Execを使用してECSをモニタリングする方法

## 1. 前提
-AWS CLIv2.13.0以降がインストールされていること 
[手順はこちら](https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/getting-started-install.html)  
-Session Manager plugin をインストールがインストールされていること。  [手順はこちら](https://docs.aws.amazon.com/ja_jp/systems-manager/latest/userguide/install-plugin-windows.html)  
-コマンド実行にはedit-[システム識別子]-uaにスイッチロールが必要です。  
-このドキュメントはpowsershelldでの実行を前提としています。
## 2. 環境変数を構成する
```env1
    $Env:HTTP_PROXY="http://proxy.isid.co.jp:8080"
    $Env:HTTPS_PROXY="http://proxy.isid.co.jp:8080"
    $Env:NO_PROXY="vpce-09abd81ced8c4d2e0-8f3miqxd.ecs.ap-northeast-1.vpce.amazonaws.com"
    $Env:AWS_ENDPOINT_URL_ECS="https://vpce-09abd81ced8c4d2e0-8f3miqxd.ecs.ap-northeast-1.vpce.amazonaws.com"
```  
※Proxyサーバーは環境によって変更して下さい。
## 3. 初回のみ実行
- ECSサービスに対しECS Execを有効にする
```cmd1
    aws ecs update-service  --cluster [クラスター名] --service [サービス名] --enable-execute-command   
```  

- タスクを強制デプロイして新しいタスクに入れ替える
```cmd2
    aws ecs update-service  --cluster [クラスター名] --service [サービス名] --force-new-deployment   
```  
※ECS Exec機能を有効化した後に起動したタスクで、はじめてECS Execが利用できます。  
※本番稼働環境では、実行に十分注意してください。

## 4. ECS Execを実行
```cmd4
    aws ecs execute-command --cluster [クラスター名] --task [タスクID]  --container [コンテナ名] --interactive --command "/bin/sh"
```  
※本番環境はセグメントルームよりアクセスできます。




