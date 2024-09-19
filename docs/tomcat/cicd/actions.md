---
date: "2024-08-24"
lastmod: "2024-08-24"
---

# CICDテンプレート利用ガイド
## 目次
- [1.CICDテンプレート適用の流れ](./index)　
- [2.GitHubActions](./actions) **←本ページ**
- [3.タスク定義ファイル](./taskdef)
- [4.アプリケーション仕様ファイル](./appspec)
<br>

---

## 2.GitHubActions
### 2-1．ワークフローファイルの作成  


#### 1. ビルドツールを使用しWar(Jar)ファイル作成  

```build
      - name: build with maven
        run: mvn --batch-mode --update-snapshots verify --file ${{env.MODULE}}/pom.xml

      - name: copy staging
        run: mkdir staging && cp ${{env.MODULE}}/target/*.war staging
```     
　環境変数には下記をセットします。  
　MODULEには、1-1※3の「poc2」をセットします。


#### 2.AWS、ECRにログイン  

```login
      - name: aws-login
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume:    ${{env.AWS_ROLE_ARN}}
          role-session-name: ${{env.AWS_SESSION_NAME}}
          aws-region:        ${{env.AWS_REGION}}

      - name: ecr-login1
        uses: aws-actions/amazon-ecr-login@v2
        id: login-ecr # outputs で参照するために id を設定
        with:
          registries: ${{env.APP_ECR_AWS_ACCOUNT}}


      - name: ecr-login2
        uses: aws-actions/amazon-ecr-login@v2
        with:
          registries: ${{env.PUBLIC_ECR_AWS_ACCOUNT}}
```     
　AWS_ROLE_ARN　　　1-3-1で作成したロールのARNをセットします  
　AWS_SESSION_NAME　任意の名前をセットします  
　AWS_REGION　　　　「ap-northeast-1」をセットします  
　APP_ECR_AWS_ACCOUNT　ECSが稼働するAWSアカウントをセットします  
　PUBLIC_ECR_AWS_ACCOUNT　「533994313413」をセットします  


#### 3. ECRにDockerImageをPush  
```ecr
      - name: build and push docker image to ecr
        id: build-image
        uses: docker/build-push-action@v6
        env:
          REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          IMAGE_TAG: ${{ github.sha }}
        with:
          context: ${{ github.workspace }}
          file: ${{env.MODULE}}/Dockerfile
          tags: ${{ env.REGISTRY }}/${{ env.ECR_REPOSITORY }}:${{ env.IMAGE_TAG }}
          push: true
```     
　MODULE　　　　　1-1※3の「poc2」をセットします  
　ECR_REPOSITORY 　ECRのリポジトリ名をセットします  


#### 5. タスク定義書き換え  
```taskdef
      - name: render new task definition
        id: task-def
        uses: aws-actions/amazon-ecs-render-task-definition@v1
        with:
          task-definition: ${{env.MODULE}}/${{env.ECS_TASK_DEFINITION}}
          container-name: ${{ env.CONTAINER_NAME }}
          image: ${{ steps.login-ecr.outputs.registry }}/${{ env.ECR_REPOSITORY }}:${{ github.sha }}
```     
　ECS_TASK_DEFINITION　　1-1※4の.aws/taskdef_dev.jsonをセットします  
　CONTAINER_NAME　　　　タスク定義のTomcatのコンテナ名をセットします  


#### 6. タスク定義の置換、ECSデプロイ  
①ローリングアップデートの場合
```deploy1
      - name: deploy ecs task definition
        uses: aws-actions/amazon-ecs-deploy-task-definition@v2
        with:
          task-definition: ${{ steps.task-def.outputs.task-definition }}
          service: ${{ env.ECS_SERVICE }}
          cluster: ${{ env.ECS_CLUSTER }}
          wait-for-service-stability: true
```
　ECS_CLUSTER　　　　ECSのクラスタ名をセットします  
　ECS_SERVICE　　　　ECSのサービス名をセットします

➁Blue/Greenの場合
```deploy2
      - name: deploy ecs task definition
        uses: aws-actions/amazon-ecs-deploy-task-definition@v2
        with:
          task-definition: ${{ steps.task-def.outputs.task-definition }}
          service: ${{ env.ECS_SERVICE }}
          cluster: ${{ env.ECS_CLUSTER }}
          codedeploy-appspec: ${{env.MODULE}}/${{env.CODE_DEP_APPSPEC}}
          codedeploy-application: ${{env.CODE_DEP_APP}}
          codedeploy-deployment-group: ${{env.CODE_DEP_GRP}}
          wait-for-service-stability: true

```
　CODE_DEP_APPSPEC　　1-1※5の「appspec_dev.yml」をセットします  
　CODE_DEP_APP　　　　CodeDeployのアプリケーション名をセットします  
　CODE_DEP_GRP　　　　CodeDeployのデプロイグループ名をセットします
<br>

| テンプレート | 説明 | 
| ------ | ------ |
| [cicd_rolling_dev.yml](/files/cicd_rolling_dev.yml) | ローリングアップデートのテンプレート | 
| [cicd_bluegreen_dev.yml](/files/cicd_bluegreen_dev.yml) | BlueGreenデプロイのテンプレート |  


<!--
<p style="margin-top: 20em"></p>  
-->
<div style="display: flex; justify-content: space-between;">
  <div style="text-align: center;">
  </div>
  <div style="text-align: center;">
　　<a href="./taskdef">3.タスク定義ファイル→</a>
  </div>
</div>

