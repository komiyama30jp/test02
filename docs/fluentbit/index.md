---
date: "2023-06-07"
lastmod: "2023-06-07"
---

# Fluent Bitテンプレート利用ガイド
## 目次
- [概要](/fluentbit/#概要)　**←このページ**
- [1.テンプレート適用の流れ](/fluentbit/#1テンプレート適用の流れ)　**←本ページ**
- [2.設定ファイルと出力](/fluentbit/configuration.html)
- [3.タスク定義の変更](/fluentbit/firelens.html)
- [4.トラブルシュート](/fluentbit/troubleshooting.html)

<br>

---

<br><br>

## 概要
Fluent Bit(フルエントビット)は、C言語で書かれたクラウド及びコンテナ環境に適した、ログの収集、配布を行うオープンソースのログプロセッサツール。  
ECSでは、ログドライバとしてCloudWatch Logsへ転送するawslogsかFireLensのいずれかを指定しなければならない。  
この時、CloudWatch LogsやS3など複数の送信先がある場合、DGCP標準監視であるDatadogにてログを監視する場合は、利用が必須となるが、非機能要件としての対応となるため、対応するための工数が発生する。  
そこで、DGCPのコンテナ標準としてFluent Bitのテンプレート設定が入ったイメージを展開することで作業を効率化することを目的にテンプレートガイドを展開する。  
![Fluent Bit概要](./files/overview.png)

<br><br>

## 1.テンプレート適用の流れ
### 1-1．設定ファイル配置場所の決定  
下記①②から選択してください。  
<br>

1. S3に配置（推奨）  
アプリ用の設定ファイルをS3に配置し、Fluent Bitが起動時に読み込む方式。  
設定ファイル配置用のS3バケットを準備してください。1.2の出力先用のバケットと併用出来ます。  
<br>

1. カスタムイメージを作成し、ローカルファイルシステムに配置  
アプリ用の設定ファイルをあらかじめ追加した、カスタムイメージを作成する方式。  
CloudWatch Logs, S3, Datadog以外に送信する場合は強制。
※サポート不可  
<br><br>

### 1-2．ログ出力先の決定
このテンプレートでは、出力先としてCloudWatch Logs、S3、Datadogに対応しています。  
標準では、CloudWatch Logsに標準ミドルウェアのログを出力します。※環境変数によってON/OFF可能  
<br>

それ以外の出力先は、下記ユースケースを元に、要件に応じて決定してください。  
<br>

1. アプリログを長期保管したい  
CloudWatch Logs, S3バケットを準備してください。  
<br>

1. アプリのログを監視したい  
CloudWatch Logs, DatadogのAPIKeyを準備（※「監視設定依頼シート」を申請）してください。  
※こちらは<span style="color: red; ">本番環境</span>のみ必要となります。
<br>

1. アプリログを長期保管、監視したい  
CloudWatch Logs, S3バケット, DatadogのAPIKeyを準備してください。  
<br>

<br><br>

### 1-3．Fluent Bitの設定ファイル作成、配置  
1.1でS3を選択した場合、「[2.設定ファイルと出力シート](/fluentbit/configuration)」を元に、設定ファイルを作成し、設定ファイル配置用のS3に配置してください。  
また、カスタムイメージ作成を選択した場合、/fluent-bit/に配置して下さい。  


<br><br>

<p style="margin-top: 20em"></p>  

| 関連ドキュメント | 説明 | 
| ------ | ------ |
| [コンテナロギング方式設計20230303.docx](/files/基本設計書/コンテナロギング方式設計20230303.docx) | コンテナのロギング（監視含む）及びログルータの設計に関して | 
| [コンテナロギング運用設計書20221121.docx](/files/基本設計書/コンテナロギング運用設計書20221121.docx) | コンテナのロギング（監視含む）及びログルータの運用設計に関して |  

<div style="display: flex; justify-content: space-between;">
  <div style="text-align: center;">
  </div>
  <div style="text-align: center;">
　　<a href="/fluentbit/configuration">2.設定ファイルと出力→</a>
  </div>
</div>