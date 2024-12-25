---
date: "2023-04-18"
lastmod: "2023-04-18"
---


## タスク定義頻出パラメータ
以下では、重要もしくは頻繁に変更するパラメータに関して抜粋し、記述する。  
タスク定義で設定可能なパラメータは[AWSリファレンス](https://docs.aws.amazon.com/ja_jp/AmazonECS/latest/developerguide/task_definition_parameters.html  
){:target="_blank"}を参考にすること。	  

- family  
タスク定義のグループ。  
ファミリーに対してリビジョンをバージョンとして連番で付与され、管理される。  
DGCPでは、「ecs-task-<環境種別:1桁>-<システム識別子:6桁>-<任意の文字列>」で設定する。  
```
"family":  "ecs-task-t-iappoc-web"
```
<br>

- requireCompatibilities  
Amazon ECS がタスク定義の検証基準となる起動タイプを指定できる。  
EC2 | FARGATE | EXTERNALから選択可。  
```
"requiresCompatibilities":  ["FARGATE"]
```
<br>

- taskRoleArn  
動作中のコンテナが利用するIAMポリシーを含んだロール。  
DGCPでは、標準で同じシステム識別子のリソース(S3など)にアクセスできる。
```
"taskRoleArn":  "arn:aws:iam::084563037095:role/rol-t-iappoc-taskrole-web"
```
<br>

- executionRoleArn  
タスクが起動する時に利用する（ECRからのイメージのPull, 環境変数の取得etc...）ロール。  
DGCPでは、標準で同じシステム識別子のECRやSSMなどにアクセスできる。
```
"executionRoleArn":  "arn:aws:iam::084563037095:role/rol-t-iappoc-taskexecuterole-web"
```
<br>

- networkMode  
タスクのコンテナで使用する Docker ネットワーキングモード。  
Fargateは「awsvpc」のみ利用できる。  
```
"networkMode":  "awsvpc"
```
<br>

- cpu  
タスクに適用される CPU ユニットのハード制限。Fargateでは必須。  
メモリと合わせて使用できる範囲が制限されている。後述のContainerDefinitions(コンテナ定義）の総量よりも多い必要がある。  
```
"cpu":  "512"
```
<br>

- memory  
タスクに適用されるメモリのハード制限。Fargateでは必須。  
CPUと合わせて使用できる範囲が制限されている。後述のContainerDefinitions(コンテナ定義）の総量よりも多い必要がある。  
```
"memory":  "1024"
```
<br>

- containerDefinitions  
コンテナの設定を定義する。  
    - name  
    コンテナの名前。  
    DGCPでは「ecs-container-<環境種別:1桁>-<システム識別子:6桁>-<任意の文字列>」で設定する。  
    ```
    "name":  "ecs-container-t-iappoc-nginx"
    ```
    <br>
    
    - image  
    ECRにアップロードしているイメージ（タグまで含む）を指定する。  
    ```
    "image":  "084563037095.dkr.ecr.ap-northeast-1.amazonaws.com/ecr-pri-t-iappoc-web:work-t-iappoc-tomcat-v1.0.0"
    ```
    <br>
    
    - cpu  
    Amazon ECS コンテナエージェントがコンテナ用に予約した cpu ユニットの数。  
    ```
    "cpu":  "512"
    ```
    <br>
    
    - memory  
    コンテナに適用されるメモリの量 (MiB 単位)。  
    コンテナ内で動作しているアプリケーションが設定値を超えるとタスクが強制停止される。  
    ```
    "memory":  "1024"
    ```
    <br>
    
    - memoryReservation  
    コンテナ用に予約するメモリのソフト制限 (MiB 単位)。  
    予約したメモリのため、超えてもタスクは強制停止されない。  
    ```
    "memoryReservation":  "512"
    ```
    <br>
    
    - essential  
    trueの場合、そのコンテナが何らかの理由で失敗または停止すると、タスクに含まれる他のすべてのコンテナは停止する。  
    ```
    "essential":  true
    ```
    <br>
    
    - stopTimeout  
    コンテナが正常に終了しなかった場合にコンテナが強制終了されるまでの待機時間 (秒)。  
    APS標準では120秒。  
    ```
    "stopTimeout":  120
    ```
    <br>
    
    - portMapping  
    コンテナはホストコンテナインスタンス上のポートにアクセスしてトラフィックを送受信するための設定。  awsvpcの場合、「containerPort」「protocol」有効となり、「hostPort」は設定しないか、同じ値を設定する。  
    ```
    "portMappings":  [
                     {
                         "containerPort":  31000,
                         "hostPort":  31000,
                         "protocol":  "tcp"
                     }
                 ]
    ```
    <br>
    
    - environment  
    コンテナに渡す環境変数の設定。  
    NameとvalueのJSON配列形式で宣言できる。  
    ```
    "environment":  [
                    {
                        "name":  "CATALINA_OPTS",
                        "value":  "-Xms512M -Xmx512M -XX:NewRatio=5"
                    }
                ]
    ```
    <br>
    
    - environmentFile  
    コンテナに渡す環境変数を含むファイルのリスト。  
    typeはs3のみサポートされ、valueはS3 ARNを設定する。  
    DGCPでは、プロキシなどを含んだ標準の環境変数ファイルが展開されている。  
    ```
    "environmentFiles":  [
                         {
                             "value":  "arn:aws:s3:::s3-t-cinfrs-fsv-common-file/ccntnr/dgcp_t_ccntnr.env",
                             "type":  "s3"
                         }
                     ]
    ```
    <br>
    
    - secrets  
    コンテナに渡す機密情報系の環境変数の設定。  
    nameに環境変数名、valueFromにSystems Manager Parameter Storeの名前又はArnを設定する。  
    ユースケースとしてはDBのユーザ、パスワードなどが該当する。  
    DGCPでは要件がない限りはSecretsManagerは利用しない。  
    Parameter Storeの設定方法は[こちら](../parameterstore/)。
    ```
    "secrets":  [
                {
                    "name":  "SECRET_INFO",
                    "valueFrom":  "/iappoc/t/SECRET_INFO"
                }
            ]
    ```
    <br>
    
    - logConfiguration  
    コンテナのログ設定の仕様。  
    awslogsで直接CloudWatchLogsに送信するか、firelens経由でログ出力する等が設定できる。  
    ```
    "logConfiguration":  {
                         "logDriver":  "awslogs",
                         "options":  {
                                         "awslogs-group":  "/dgcp/apl/t-irognz/ecs-task-t-iappoc-web",
                                         "awslogs-region":  "ap-northeast-1",
                                         "awslogs-stream-prefix":  "tomcat"
                                     }
                     }
    ```