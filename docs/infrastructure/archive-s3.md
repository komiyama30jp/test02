---
title: アーカイブ用S3
last_modified_date: "2025-03-19"
parent: インフラテンプレート利用ガイド
nav_order: 5
---

## 目的

`電通Security Policy（システムチェックシート等）`では、`13ヶ月`のログ保管を求められるため、  
ライフサイクルポリシーやストレージクラスを標準化し、ログ保管先として最適なアーカイブ用の`s3`のテンプレートを提供します。

## 概念図  
<br>
  
![アーカイブ用S3](./files/archive-s3.svg)  
<br>

## 提供テンプレート(CloudFormation)

| ファイル名 | 用途 | 保存先<br>(S3 URL) | 
| --- | --- | --- |
| [archive-s3-template.yml](https://ap-northeast-1.console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/quickcreate?templateURL=https://dgcp-container-guide-311141522354.s3.ap-northeast-1.amazonaws.com/deploy/infrastructure/files/template/archive-s3-template.yml){: target="_blank"} | アーカイブ用S3バケットの構築 | `https://dgcp-container-guide-311141522354.s3.ap-northeast-1.amazonaws.com/deploy/infrastructure/files/template/archive-s3-template.yml` |

## 入力パラメータ

| 入力項目 | 設定値 | デフォルト値 | 備考 | 
| --- | --- | --- | --- |
| SystemId | システム識別子 | N/A |  |
| EnvType | 環境種別:1桁 | N/A |  |
| BillingID | DGCPから払い出された値 | N/A | |
| KMSKeyArn | KMSキーのARN | N/A | KMS暗号化は、情報区分「`極秘・取り扱い厳重注意`」の時のみ必須 |
| BucketNameSuffix | S3バケット名のサフィックス | `log-archive` | S3バケットはグローバルで一意となるようにすること |
| S3StorageClass | ストレージクラス | `STANDARD_IA` | [Amazon S3 ストレージクラス](https://aws.amazon.com/jp/s3/storage-classes/){:target="_blank"} |
| S3TransitionDate | 経過日数に伴うストレージクラスへの移行日  | `90` |  |
| S3ExpireDate | 経過日数に伴うオブジェクトの削除日 | `403` | 31days * 13month = 403days |

{: .note}  
ストレージクラスは、[参考チャート](https://dev.classmethod.jp/articles/should_i_choice_s3_storage_class_2023/#toc-s3-1){: target="_blank"} を元に変更してもOKです。  

## 作成リソース

| リソース種別 | 名前 | 備考 | 
| --- | --- | --- |
| S3 Bucket | s3-`環境種別`-`システム識別`-`任意の名前` |  |
| S3 LifeCycle Policy | `TransitionIn`-`ストレージクラス`-`移行日`Days | 経過日数に伴うストレージクラスへの移行日<br>`Default: 90` |
| S3 LifeCycle Policy | `ExpirationIn`-`オブジェクト削除日`Days | 経過日数に伴うオブジェクトの削除日<br>`Default: 403` |