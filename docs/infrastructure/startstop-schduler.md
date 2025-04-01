---
title: ECSサービス定期起動停止
last_modified_date: "2025-02-20"
parent: インフラテンプレート利用ガイド
nav_order: 3
---

## 目的

`非本番環境`などで`24h365d`で稼働し続ける必要がないケースが多く存在します。  
従って費用削減に寄与するため、定期的に`ECSサービスのタスク数`を増減できる仕組みを提供します。  

{: .note}  
本テンプレートは、`タスク数`を`2（起動）`、`0（停止）`と増減するようにしていますが、  
カスタマイズすることでピーク時間帯は`3`、それ以外は、`1`とスケールイン・アウトの用途として利用することもできます。

## 概念図  
<br>
  
![ECSサービス定期起動停止](./files/sche-startstop.svg)  
<br>

## 提供テンプレート(CloudFormation)

| ファイル名 | 用途 | 保存先<br>(S3 URL) | 
| --- | --- | --- | --- |
| [ecs-service-startstop-schduler-template.yml](https://ap-northeast-1.console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/quickcreate?templateURL=https://dgcp-container-guide-311141522354.s3.ap-northeast-1.amazonaws.com/deploy/infrastructure/files/template/ecs-service-startstop-schduler-template.yml){: target="_blank"} | ECSサービス定期起動停止の構築 | `https://dgcp-container-guide-311141522354.s3.ap-northeast-1.amazonaws.com/deploy/infrastructure/files/template/ecs-service-startstop-schduler-template.yml` |

## 入力パラメータ

| 入力項目 | 設定値 | デフォルト値 | 備考 | 
| --- | --- | --- | --- |
| StartServiceTime | cron式 | `cron(0 9 * * ? *)` | 起動時間<br>デフォルトは、`毎日 9時`に起動 |
| StopServiceTime | cron式 | `cron(0 18 * * ? *)` | 停止時間<br>デフォルトは、`毎日 18時`に停止 |
| TimeZone | `Asia/Tokyo` | `Asia/Tokyo` | - |
| DesiredCount | ECSサービスのタスク数 | `2` | タスク数は、必要に応じて増減すること |
| ECSClusterName | 対象のECSクラスター名 | N/A |  |
| ECSServiceName | 対象のECSサービス名 | N/A |  |

## 作成されるリソース

| リソース種別 | 名前 | 備考 | 
| --- | --- | --- |
| EventBridge ScheduleGroup | ScheGrp-StartStop-`ECSサービス名` |  |
| EventBridge Schedule | Sche-Start-`ECSサービス名` | 起動するスケジュール |
| EventBridge Schedule | Sche-Stop-`ECSサービス名` | 停止するスケジュール |
| IAM Role | `スタック名`-EventBridgeSchedulerRole-`スタックID` |  |