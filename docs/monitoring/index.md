---
date: "2024-10-30"
lastmod: "2024-10-30"
---

# ECS Execを使用してECSをモニタリングする方法

1. ECSサービスに対しECS Execを有効にする
  
```cmd1
    aws ecs update-service  --cluster [クラスター名] --service [サービス名] --enable-execute-command   
```  
※コマンド実行前にはedit-[システム識別子]-uaにスイッチロールが必要です。

1. タスクを強制デプロイして新しいタスクに入れ替える
```cmd2
    aws ecs update-service  --cluster [クラスター名] --service [サービス名] --force-new-deployment   
```  
※コマンド実行前にはedit-[システム識別子]-uaにスイッチロールが必要です。  
※ECS Exec機能を有効化した後に起動したタスクで、はじめてECS Execが利用できます。  
※本番稼働環境では、実行に十分注意してください。

1. Proxyを構成する。
```cmd3
    $Env:HTTP_PROXY="http://proxy.isid.co.jp:8080"
    $Env:HTTPS_PROXY="http://proxy.isid.co.jp:8080"
    $Env:NO_PROXY="vpce-09abd81ced8c4d2e0-8f3miqxd.ecs.ap-northeast-1.vpce.amazonaws.com"
```  
※Proxyサーバーは環境によって変更して下さい。

1. ECS Execを実行する。

```cmd4
    aws ecs execute-command --cluster [クラスター名] --task [タスクID]  --container [コンテナ名] --interactive --command "/bin/sh" --endpoint-url https://vpce-09abd81ced8c4d2e0-8f3miqxd.ecs.ap-northeast-1.vpce.amazonaws.com
```  
※コマンド実行にはedit-[システム識別子]-uaにスイッチロールが必要です。  
※本番環境はセグメントルームよりアクセスできます。




