---
date: "2024-08-24"
lastmod: "2024-08-24"
---

# CICDテンプレート利用ガイド
## 目次
- [1.CICDテンプレート適用の流れ](/fluentbit/#1テンプレート適用の流れ)　
- [2.GitHubActions](/cicd/actions)
- [3.タスク定義ファイル](/cicd/taskdef)
- [4.アプリケーション仕様ファイル](/cicd/appspec)　**←本ページ**
<br>

---

## 4.アプリケーション仕様ファイル(appspec.yaml)

### 4-1．appspec.yamlの作成

1. Blue/Greenを選択した場合  

```yml
version: 1
Resources:
- TargetService:
    Type: AWS::ECS::Service
    Properties:
        TaskDefinition: "Placeholder: GitHub Actions will fill this in"
        LoadBalancerInfo: 
            ContainerName: "tomcat"
            ContainerPort: 31100
```

ContainerNameには、タスク定義のコンテナ名を記述してください。  
この例では<span style="color: red; ">tomcat</span>です。  

### 4-2．githubに配置
作成したappspec.yamlをgithubにPushします(前頁の※5)


<br>

<!--
<p style="margin-top: 20em"></p>  
-->
<div style="display: flex; justify-content: space-between;">
  <div style="text-align: center;">
  </div>
  <div style="text-align: center;">
　　<a href="./index">1.CICDテンプレート適用の流れ→</a>
  </div>
</div>


