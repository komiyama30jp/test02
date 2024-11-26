---
date: "2024-08-24"
lastmod: "2024-10-03"
---

# CICDテンプレート利用ガイド
## 目次
- [1.CICDテンプレート適用の流れ](./index.html)　**←本ページ**
- [2.GitHubActions](./actions.html) 
- [3.タスク定義ファイル](./taskdef.html)
- [4.アプリケーション仕様ファイル](./appspec.html)
<br>

---

## 1.CICDテンプレート適用の流れ
### 1-1．本ドキュメントの例  
#### githubの階層  
このテンプレートで使用するgithubのファイル構造は下記の通りとします。  
このテンプレートでは、環境ごとに必要なファイルは、「_dev」としています。  
環境毎のファイルは適宜作成してください。
```github
    root  
      +-Dockerfile              ※1
      +-pom.xml                 ※2
      |  
      +-.github/workflows  
      |  +-cicd_rolling_dev.yml    ローリングアップデート用のGitHubActiosのファイル
      |  +-cicd_bluegreen_dev.yml  BlueGreenデプロイ用のGitHubActiosのファイル
      +-poc2                    ※3アプリのプロジェクト
         +-.src  
         |  +...  
         +-.aws  
           +-taskdef_dev.json     ※4
           +-appspec_dev.yml      ※5
```     

### 1-2．デプロイ方式の決定  
Blue/Greenデプロイメントかローリングアップデートのどちらの方式を採用するか決定してください。  
デプロイ方式の詳細は[【ECS】コンテナデプロイ方式検討.xlsx ](https://esq365.sharepoint.com/:x:/s/msteams_30dfd0/Ee8N9W8uFx5PuPZCnjEEAQMB9B2EoO3yN95imlYjEaFy_g?e=tsRFZN)を参照してください。
<br>
1. ローリングアップデート(標準デプロイ方式)を選択した場合  
<br>
下記2つのファイルを環境別に準備します。

    ①   Dockerfile  
    ベースとなるイメージに対し、どのような操作をするかを記したファイル。  
    基本的な話なので、詳細割愛。  
<br>
    ➁   タスク定義  
    デプロイするコンテナイメージ、コンテナに割り当てるリソース、IAMロール、CloudWatchLogsの出力先等を指定する  
    定義ファイル。ファイル名はtaskdef_dev.jsonとして保存します。

1. Blue/Greenを選択した場合  
<br>
上記①②に加え下記を準備します。  
<br>
    ③   アプリケーション仕様ファイル(appspec_dev.yml)  
    どのサービスをデプロイするか、どの定義を元にどこのインフラへリリースするかを定義したファイル。  
    ファイル名はappspec_dev.ymlとします。
<br>


### 1-3．GitHubActionsの構成ファイルを作成
下記のフローに沿ってテンプレートを修正します。 詳細は次の章で説明します。 
1. ビルドツールを使用しWar(Jar)ファイル作成
1. AWS、ECRにログイン  
1. ECRにDockerImageをPush  
1. タスク定義書き換え  
1. タスク定義書置換、ECSデプロイ  

### 1-3．AWSにロールを作成
１. IAMロールを作成し、GitHubのリポジトリとの信頼関係を作成します。  

ロール名
```Role
ロール名：rol-[環境識別子]-[システム識別子]-githubactions
```  
信頼関係
```
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::[AWSアカウント]:oidc-provider/token.actions.githubusercontent.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringLike": {
                    "token.actions.githubusercontent.com:sub": [
                        "repo:dentsusoken/[リポジトリ名]:*"
                    ]
                }
            }
        }
    ]
}
```  
 ２. 作成したロールに、pol-dgcp-write-oidc-all、pol-dgcp-write-oidc-all_2のポリシーをアタッチしてください。
<br>



<!--
<p style="margin-top: 20em"></p>  
-->
<div style="display: flex; justify-content: space-between;">
  <div style="text-align: center;">
  </div>
  <div style="text-align: center;">
　　<a href="./actions">2.GitHubActions→</a>
  </div>
</div>




