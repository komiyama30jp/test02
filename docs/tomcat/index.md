---
date: "2023-04-25"
lastmod: "2023-06-02"
---

## ローカル開発

## Pleiades導入手順
1. [公式サイト](https://mergedoc.osdn.jp/)からPleiades（Java版）を取得します。  
※Mavenを使用する場合、Eclipse2022は不具合があるためEclipse2023以降を選択してください。  
    ![download](./files/tom001.png)  
2. ダウンロードした、ファイルを実行し任意の場所に解凍してください。  
    ![解凍](./files/pleiades_001.png)  
※このドキュメントは「D:\pleiades\2023-03」に解凍し、以降このディレクトリで説明します。  
3. eclipseを起動します。  
 D:\pleiades\2023-03\eclipse.exeを実行し、続いてワークスペースを作成します。  
    ![実行](./files/pleiades_002.png)  

    ![workspace](./files/pleiades_003.png)  

4. Maven使用時のプロキシを設定します。  
- &ensp;ウインドウ → 設定 → Maven → ユーザー設定を選択します。  
    ![maven](./files/pleiades_010.png)  
- &ensp;ユーザー設定を確認し、デフォルトで上記のような設定になっているのですが、settings.xml  
は存在していないので新規に作成します。  
    ![maven_dir](./files/pleiades_011.png)  

- &ensp; settings.xmlの記載内容は下記の通りに記載してください。<br>　※SNET内の端末で作業する場合  


```settings.xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.1.0 http://maven.apache.org/xsd/settings-1.1.0.xsd">
  <proxies>
    <proxy>
      <id>http_proxy</id>
      <active>true</active>
      <protocol>http</protocol>
      <host>proxy.isid.co.jp</host>
      <port>8080</port>
    </proxy>
    <proxy>
      <id>https_proxy</id>
      <active>true</active>
      <protocol>https</protocol>
      <host>proxy.isid.co.jp</host>
      <port>8080</port>
    </proxy>
  </proxies>
</settings>
```