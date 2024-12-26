---
date: "2023-03-29"
lastmod: "2024-11-07"
---

# CIT基幹BU コンテナ開発標準
## 目次
### 概要
- [本ドキュメントの対象者](#本ドキュメントの対象者)
- [コンテナ概要](#コンテナ概要)
- [従来の開発との差異](#従来の開発との差異)

### 開発・実装
- [ローカル開発環境](#ローカル開発環境)
- [コンテナ詳細設計指針](#コンテナ詳細設計指針)
- [他ランタイム環境の構築手順](#他ランタイム環境の構築手順)

### AWS環境構築
- [デプロイ方式](#デプロイ方式)
- [コンテナログ方式](#コンテナログ方式)
- [ツール一覧](#ツール一覧)

### その他
- [認証認可](#認証認可)
- [FAQ](#FAQ)
- [Appendix-設計書一覧](#appendix-設計書一覧)

---
<br>

## 概要
### 本ドキュメントの対象者
- コンテナは聞いたことがある or イマイチ分からない方
- 従来型の仮想VMの開発とコンテナ開発の何が異なるのか把握していない方
- EC2から移行したい方
- デプロイや認証方式に関して、標準やテンプレートに則りたい方

<br><br>

### コンテナ概要
[コンテナとは](/container/about/)　 　 　…コンテナ/AWSが分からない方向け。コンテナ～AWSサービスを記載  
[コンテナ利用フロー](/flow/)　…DGCPでコンテナ開発をするにあたり必要な申請をフローで記載

<br><br>

### 従来の開発との差異
[EC2等の仮想(vm)マシンとの差異](/comp/main/)　 　 　…VMとコンテナ、企画~運用フェーズなどの差異。費用比較などを記載。PM/開発者両方向け。  
[通常のTomcatと組込Tomcatの開発者視点での差異](/comp/tomcat/)　 　 　…構造や仕様上の差異について記載。開発者向け。 

---
<br><br>

## 開発・実装
### ローカル開発環境
[IDE導入手順](/local-env/pleiades/)　　…Java開発のローカルIDEの構築手順のテンプレート  
[egit連携](/local-env/egit/)　　　…電通総研Org gitとの連携方法  
[Docker Desktopインストール手順](/local-env/docker-desktop/)　　…DockerDesktopインストール手順  
<br><br>

### コンテナ詳細設計指針
[コンテナ詳細設計指針](/guideline/)　　…Spring Boot3及びJDK17を前提としたコンテナ設計における指針及び指針に従ったサンプルアプリケーションの提供  
[DGCP共通モジュール](/dgcpmodule/)    …Spring Boot3に対応したDGCP標準設定の適用モジュール。機能や利用手順を記載  

<br><br>

### 他ランタイム環境の構築手順
[batch](/container/batch/)　　…linuxのsh/bash等を利用したバッチコンテナの作成手順  
[nodejs](/container/nodejs/)　　…nodejsコンテナの作成手順  
[python](/container/python/)　　…pythonコンテナの作成手順  

---
<br><br>

## AWS環境構築

### デプロイ方式
[手動デプロイガイド](/deploy/manual/)　　　…CLIまたは管理コンソールによるアプリケーションのデプロイ方法に関して  
[CICDテンプレート利用ガイド](/deploy/cicd/)　　　　　…GitHub Actionsを利用したCICDラインの構築に関してのドキュメント。コンテナ標準へのJavaデプロイ方法を記載。  

<br><br>

### コンテナログ方式
[Fluentbitテンプレート利用ガイド](/fluentbit/)　　…コンテナロギング（ログ監視含む）を設定する場合の標準fluentbitに利用方法等に関して

<br><br>

### ツール一覧
[okta-awscli](/tools/okta-awscli/)　　　　　　　…DGCPから払い出されるIAMロールを利用したCLI操作を可能にするps1スクリプト  
[標準ログの平文形式取得](/tools/logs/)　　…コンテナ標準チームのMW（Nginx/Tomcat）のJSON形式のログをCWLogsから平文として取得するps1スクリプト  

**※利便性向上を目的に作成したツールを公開しておりますため事前予告なしに仕様の変更等発生いたします。<br>自己責任にて、ご利用ください。**

---
<br><br>



<p style="margin-top: 20em"></p>

## その他

### 認証認可
[Keycloak標準化検討.xlsx](https://esq365.sharepoint.com/:x:/s/msteams_30dfd0/Eak_a9k9UV9OgaSpj341N84BoCpOZWSmfCt0DDiG92R3CA?e=jbFNGN){:target="_blank"} 　　…CASで採用されたアイデンティティブローカーとしてのKeycloakに関する標準化検討に関したドキュメント  
><ins>結論</ins>  
Oktaでの認証・認可を採用するべきとし、Keycloakの標準化は実施しない。

<br><br>
### FAQ
[よくある質問](./faq/)
<br><br>

### Appendix-設計書一覧

| ファイル名 | 説明 | 
| ------ | ------ |
| [コンテナAPサーバ基本設計書(Tomcat9)](https://esq365.sharepoint.com/:w:/s/msteams_30dfd0/Eb3nrodevExDve8Psahb2B8BF07aOOE2VscTo8QKuJc4-A?e=Vlvuuk){:target="_blank"} | APS標準のベースイメージNginx/Tomcatを用いたサーバ構成について<br><u>Tomcat9 & JDK8のドキュメント</u> | 
| [【Tomcat】コンテナ設定標準化(Tomcat9)](https://esq365.sharepoint.com/:x:/s/msteams_30dfd0/EejCoiZ7Ho9ArISKeX3naBIBmc4iYom_OqtV74mymmgsUg?e=CbB7qh){:target="_blank"} | APS標準のベースイメージのTomcatの設定に関して<br><u>Tomcat9 & JDK8のドキュメント</u> | 
| [コンテナ標準クラスパス-JavaOption検討(Tomcat9)](https://esq365.sharepoint.com/:x:/s/msteams_30dfd0/EaanfZtQG2hClQ4FV-pKcAUBTqVn0U1aIqwKPffHACRTiQ?e=bnZdDh){:target="_blank"} | APS標準のベースイメージのJVM（Tomcat）上で設定すべき項目<br><u>Tomcat9 & JDK8のドキュメント</u> | 
| [コンテナ標準ディレクトリ構成(Tomcat9)](https://esq365.sharepoint.com/:x:/s/msteams_30dfd0/EYZXXN3CuE5Jn33qFt3PDR0BYJQbKFUQvNT5My6GnmN3Gg?e=a2f2Yl){:target="_blank"} | APS標準のベースイメージのディレクトリ構成<br><u>Tomcat9 & JDK8のドキュメント</u> | 
| [コンテナAPサーバ基本設計書(Tomcat10)](https://esq365.sharepoint.com/:w:/s/msteams_30dfd0/EZ99XdlWIU9FgbDJfUFARscBYA4MIs3j8u0XQ4hfx5BsFA?e=vkchpC){:target="_blank"} | APS標準のベースイメージNginx/Tomcatを用いたサーバ構成について<br><u>Tomcat10 & JDK17のドキュメント</u> | 
| [【Tomcat】コンテナ設定標準化(Tomcat10)](https://esq365.sharepoint.com/:x:/s/msteams_30dfd0/ESRkwLHxsg1MnwO34CnLmLcB4q4JFqSLggykMtC9dCQ9Pg?e=mKaUA9){:target="_blank"} | APS標準のベースイメージのTomcatの設定に関して<br><u>Tomcat19 & JDK17のドキュメント</u> | 
| [コンテナ標準クラスパス-JavaOption検討(Tomcat10)](https://esq365.sharepoint.com/:x:/s/msteams_30dfd0/EZS_pyuhQDtIuhZB96j8CJQBM40LtRetVxIIkjOVSthapw?e=5r1Woo){:target="_blank"} | APS標準のベースイメージのJVM（Tomcat）上で設定すべき項目<br><u>Tomcat10 & JDK17のドキュメント</u> | 
| [コンテナ標準ディレクトリ構成(Tomcat10)](https://esq365.sharepoint.com/:x:/s/msteams_30dfd0/EY0M3Ya1tWZJmvlGcCvGE3ABBwZRNsqEDYuY-mP9aRnRbQ?e=jyLpgh){:target="_blank"} | APS標準のベースイメージのディレクトリ構成<br><u>Tomcat10 & JDK17のドキュメント</u> | 
| [コンテナAPサーバ運用設計書](https://esq365.sharepoint.com/:w:/s/msteams_30dfd0/ETwYVTUNtNZMhBMRqtXToqEBGLx9gZBBn0yTRjMi5zdAzA?e=E04O8K){:target="_blank"} | APS標準のベースイメージの運用に関して | 
| [コンテナロギング方式設計](https://esq365.sharepoint.com/:w:/s/msteams_30dfd0/EbufU_JCsEFNtayvpgGMIowBG97_DHolyDLmmk2FRePmZA?e=A3EvJT){:target="_blank"} | コンテナのロギング（監視含む）及びログルータの設計に関して | 
| [コンテナロギング運用設計書](https://esq365.sharepoint.com/:w:/s/msteams_30dfd0/EdINJhuysMZAriJCGS-IW5sB-6a1xXJWwfnc4we3fNanTA?e=rPr6yR){:target="_blank"} | コンテナのロギング（監視含む）及びログルータの運用設計に関して | 
| [CICD基本設計書(コンテナJava開発者向け)](https://esq365.sharepoint.com/:w:/s/msteams_30dfd0/EeBWgL5I-SlNuenwE2FmRa4BGUoCA7nSReBfC4eL_jo5dA?e=S0stCR){:target="_blank"} | コンテナのロギング（監視含む）及びログルータの設計に関して | 
| [【Nginx】コンテナ設定標準化](https://esq365.sharepoint.com/:x:/s/msteams_30dfd0/ESh_VLO9xhNDmNcsNW3WaEABqEnLpGu9LbPJOfv-kiIAiA?e=4PzX2t){:target="_blank"} | APS標準のベースイメージのNginxの設定に関して | 
| [【Nginx・Tomcat】コンテナログ設計書](https://esq365.sharepoint.com/:x:/s/msteams_30dfd0/ESJPOuxq0LdMuFAvAw8PdHwBPI5l-F3AQkaY_dSHZQBQtw?e=1VkBFQ){:target="_blank"} | APS標準のベースイメージ標準出力（エラー含む）されるログに関して | 

### Appendix-方式/検証（検討）
設計以外にもコンテナチームにて検証、作成している以下のようなドキュメントが存在する。
* 【ECS】GracefulShutdown
* 【ECS】デプロイ方式
* 【ECS】タスク再作成の判定検証
* 【ECS】Tomcatメモリーサイジング検討
* 【ECS】メモリ制限方式検討

[こちら](https://esq365.sharepoint.com/:f:/s/msteams_30dfd0/EuSyEXMuGL1MioaofygGr4wB4H59oZ5EUrTNdOExQx342Q?e=p9YmeE){:target="_blank"}を参照すること。

---

