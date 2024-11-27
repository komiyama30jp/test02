---
date: "2023-06-07"
lastmod: "2023-06-07"
---

# Fluent Bitテンプレート利用ガイド
## 目次
- [概要](/fluentbit/#概要)
- [1.テンプレート適用の流れ](/fluentbit/#1テンプレート適用の流れ)
- [2.設定ファイルと出力](/fluentbit/configuration.html)
- [3.タスク定義の変更](/fluentbit/firelens.html)
- [4.トラブルシュート](/fluentbit/troubleshooting.html)　**←本ページ**
<br>

---

<br><br>

## 4. トラブルシュート

| No | 現象 | 原因 | 対応 |
| ------ | ------ | ------ | ------ |
| 1 | Datadogにログが出力されない | タスク定義に環境ファイルが設定されていない | 「3.4ログコンテナ環境ファイルの追加」を参照し<br>環境ファイルが正しく設定できているか確認してください。 |
| 2 | Datadogにログが出力されない | APIKeyに誤った文字列が設定されている | firelensのログを確認し<br>「https://http-intake.logs.datadoghq.com:443 HTTP status=403」<br>が出力されている場合、監視チームに問い合わせを行ってください。 |
| 3 | Firelensコンテナが起動しない | タスク定義の環境ファイルのS3 ARNが間違っている | タスクの停止理由に<br>「Resourceinitializationerror: failed to download env files」<br>と出力されている場合<br>「3.4ログコンテナ環境ファイルの追加」を参照し<br>環境ファイルが正しく設定できているか確認してください。 |
| 5 | Firelensコンテナが起動しない | AWS管理コンソール上での入力時に改行コードや制御コードが誤って挿入されている。 | タスク定義のJson形式で「？」が入っていないか確認し、<br>入っている場合は削除してください。 |
| 6 | 設定ファイルがS3から取得できない。 | タスク定義に環境変数CONF_BUCKET_DIRが設定されていないまたは、誤った文字列が設定されている。 | firelensのログを確認し、<br>「--------apl_xxxxxx_xx.conf doesn't exist.--------」<br>が出力されている場合、<br>環境変数が正しく設定できているか確認してください。<br>両端に誤って空白が挿入されることがあります。取り除いてください。 |


<br><br>

<p style="margin-top: 20em"></p>  

| 関連ドキュメント | 説明 | 
| ------ | ------ |
| [コンテナロギング方式設計20230303.docx](/files/基本設計書/コンテナロギング方式設計20230303.docx) | コンテナのロギング（監視含む）及びログルータの設計に関して | 
| [コンテナロギング運用設計書20221121.docx](/files/基本設計書/コンテナロギング運用設計書20221121.docx) | コンテナのロギング（監視含む）及びログルータの運用設計に関して |  

<div style="display: flex; justify-content: space-between;">
  <div style="text-align: center;">
    <a href="/fluentbit/firelens">←3.タスク定義の変更</a>
  </div>
  <div style="text-align: center;">
　
  </div>
</div>