---
date: "2023-06-07"
lastmod: "2023-06-07"
---

# Fluent Bitテンプレート利用ガイド
## 目次
- [概要](/fluentbit/#概要)
- [1.テンプレート適用の流れ](/fluentbit/#1テンプレート適用の流れ)
- [2.設定ファイルと出力](/fluentbit/configuration.html)　**←本ページ**
- [3.タスク定義の変更](/fluentbit/firelens.html)
- [4.トラブルシュート](/fluentbit/troubleshooting.html)

<br>

---

<br><br>

## 2.設定ファイルと出力
### 2-1．ログ出力  
下記のサンプルのようにlogName, level, messageの3つの項目を持つログをJson形式で出力します。  
以降、このログを使用し説明します。

```sh
# ログサンプル
{
	"esqid": "lis15k",
	"logName": "api_trace",
	"level": "warn",
	"message": "エラーメッセージテスト。"
}
```  
<br>

### 2-2．StreamProcessorの設定ファイル作成  
1. apl_stream_processor.confという名前のファイルを作成します。
1. apl_stream_processor.confに下記サンプルを元に設定を記載します。  
<br>
この例ではlogNameがapl_traceのログを「\*-firelens-\*」というタグ（標準仕様）が付与されたStreamより抽出します。  
<span style="color: red; ">apl_trace</span>という名前、<span style="color: red; ">apl_trace-log</span>というタグ名で新規にStreamを作成します。  

```sh
# apl_stream_processor.conf
[STREAM_TASK]
    Name api_trace
    Exec CREATE STREAM api_trace WITH (tag='api-trace-log') AS SELECT * FROM TAG:'*-firelens-*' WHERE logName = 'api_trace' ;
```

※「apl-trace」の名前は任意  
<br>

### 2-3．CloudWatch Logsへの出力設定作成  
1. apl_output_cloudwatch.confという名前のファイルを作成します。
1. apl_output_cloudwatch.confに下記サンプルを元に設定を記載します。  
<br>
この例では<span style="color: red; ">apl_trace</span>というタグが付与されたStreamを、  
ロググループ${LOG_GROUP_NAME}へapi-trace-logという名前のprefixを付けてログストリームへ送信します。  

```sh
# apl_output_cloudwatch.conf
[OUTPUT]		
    Name   cloudwatch		
    Match  api-trace-log		
    region ${AWS_REGION}		
    log_group_name ${LOG_GROUP_NAME}		
    log_stream_name api-trace-log/$(ecs_task_id)		
```

※「apl-trace-log」の名前は任意  
![CloudWatch Logs例](./files/output_cw.png)  
<br>

### 2-4．S3への出力設定作成  
1. apl_output_s3.confという名前のファイルを作成します。
1. apl_output_s3.confに下記サンプルを元に設定を記載します。  
<br>
この例では<span style="color: red; ">apl_trace</span>というタグが付与されたStreamを、  
バケット名 ${LOG_BUCKET_NAME}へ送信します。

```sh
# apl_output_s3.conf
[OUTPUT]	
    Name s3	
    Match  api-trace-log	
    region ${AWS_REGION}	
    bucket ${LOG_BUCKET_NAME}	
    json_date_key false	
    total_file_size 1M	
    upload_timeout 1m	
    s3_key_format /$TAG/%Y/%m/%d/%H/%M/%S-$UUID.gz	
    use_put_object On	
    compression gzip	
    content_type application/gzip	
	
```

※「apl-trace-log」の名前は任意  
![S3例](./files/output_s3.png)  
<br>

### 2-5．Datadogへの出力設定作成  

※この設定は<span style="color: red; ">本番環境</span>のみ必要となります。    

1. apl_output_dd.confという名前のファイルを作成します。
1. apl_output_dd.confに下記サンプルを元に設定を記載します。  
<br>
この例では<span style="color: red; ">apl_trace</span>というタグが付与されたStreamを、  
${DD_API_KEY}を発行したOrganizationへ送信します。
※「監視設定依頼シート」を記載し、監視チームに申請すると発行頂けます。

```sh
# apl_output_dd.conf
[OUTPUT]		
    Name        datadog		
    Match       api-trace-log		
    Host        http-intake.logs.datadoghq.com		
    TLS         on		
    apikey      ${DD_API_KEY}		
    dd_service  ecs		
    compress    gzip		
    include_tag_key true		

```

※「apl-trace-log」の名前は任意  
この時、Datadogはログ中のlevel, messageによって下記のように処理を行います。  

| level | 処理内容 | 
| ------ | ------ |
| info | 処理しない |
| warn | 警戒域の処理を行う（デフォルトmessage:*） |
| error | 危険域の処理を行う（デフォルトmessage:*） |

![DD例](./files/output_dd.png)  
<br>

<br><br>

<p style="margin-top: 20em"></p>  

| 関連ドキュメント | 説明 | 
| ------ | ------ |
| [コンテナロギング方式設計20230303.docx](/files/基本設計書/コンテナロギング方式設計20230303.docx) | コンテナのロギング（監視含む）及びログルータの設計に関して | 
| [コンテナロギング運用設計書20221121.docx](/files/基本設計書/コンテナロギング運用設計書20221121.docx) | コンテナのロギング（監視含む）及びログルータの運用設計に関して |  

<div style="display: flex; justify-content: space-between;">
  <div style="text-align: center;">
    <a href="/fluentbit/#1テンプレート適用の流れ">←1.テンプレート適用の流れ</a>
  </div>
  <div style="text-align: center;">
　　<a href="/fluentbit/firelens">3.タスク定義の変更→</a>
  </div>
</div>