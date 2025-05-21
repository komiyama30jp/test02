---
title: トラブルシュート
last_modified_date: "2025-02-19"
nav_order: 3
parent: Fluent Bitテンプレート利用ガイド
---

# Fluent Bitテンプレート利用ガイド

## 4. トラブルシュート

| 現象 | 原因 | 対応 |
| ------ | ------ | ------ |
| `Datadog`にログが出力されない | タスク定義に環境ファイルが設定されていない | [3.4ログコンテナ環境ファイルの追加](/fluentbit/firelens.html#3-4firelensコンテナに環境ファイルを追加)を参照し、<br>環境ファイルが正しく設定できているか確認してください。 |
| `Datadog`にログが出力されない | `APIKey`に誤った文字列が設定されている | firelensのログを確認し、<br>`https://http-intake.logs.datadoghq.com:443 HTTP status=403`<br>が出力されている場合、監視チームに問い合わせを行ってください。 |
| Firelensコンテナが起動しない | タスク定義の環境ファイルの`S3 ARN`が間違っている | タスクの停止理由に<br>`Resourceinitializationerror: failed to download env files`<br>と出力されている場合<br>[3.4ログコンテナ環境ファイルの追加](/fluentbit/firelens.html#3-4firelensコンテナに環境ファイルを追加)を参照し、<br>環境ファイルが正しく設定できているか確認してください。 |
| Firelensコンテナが起動しない | AWS管理コンソール上での入力時に改行コードや制御コードが誤って挿入されている。 | タスク定義の`Json形式`で「`？`」が入っていないか確認し、<br>入っている場合は削除してください。 |
| 設定ファイルが`S3`から取得できない。 | タスク定義に環境変数`CONF_BUCKET_DIR`が設定されていないまたは、誤った文字列が設定されている。 | firelensのログを確認し、<br>`--------apl_xxxxxx_xx.conf doesn't exist.--------`<br>が出力されている場合<br>環境変数が正しく設定できているか確認してください。<br>両端に誤って空白が挿入されることがあります。取り除いてください。 |


<br><br>

<p style="margin-top: 20em"></p>  

| 関連ドキュメント | 説明 | 
| ------ | ------ |
| [コンテナロギング方式設計20230303.docx](/files/基本設計書/コンテナロギング方式設計20230303.docx) | コンテナのロギング（監視含む）及びログルータの設計に関して | 
| [コンテナロギング運用設計書20221121.docx](/files/基本設計書/コンテナロギング運用設計書20221121.docx) | コンテナのロギング（監視含む）及びログルータの運用設計に関して |  

<div style="display: flex; justify-content: space-between;">
  <div style="text-align: center;">
    <a href="/deploy/fluentbit/firelens">←3.タスク定義の変更</a>
  </div>
  <div style="text-align: center;">
　
  </div>
</div>