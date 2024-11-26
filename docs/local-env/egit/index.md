---
date: "2023-03-29"
lastmod: "2024-11-06"
---
# ローカル開発 ～egit連携～

## egit連携
1. ローカルIDEの確認  
    1. eclipseのプラグインでegitが反映されている状態を確認する。  
    ![パースペクティブ](./files/egit_014.png)  
    ※egitプラグインが入っていない場合は、適用してください。  
    2. GitHubから取得するソースの管理ディレクトリを作成  
    eclipseのディレクトリ内にGitHubソースの管理ディレクトリを作成します（任意の場所、フォルダ名で可）。  
    例）　D:\eclipse\git  
    ![管理ディレクトリ](./files/egit_001.png)  
1. パーソナルアクセストークンの作成  
    1.  GitHubにログインし、Settings を選択する。  
        https://github.com/  
    ![Settings](./files/egit_002.png)  
    1. Developer Settings を選択する。  
    ![DeveloperSettings](./files/egit_003.png)  
    1. Personal access tokens -  token(classic) を選択する。  
    ![PAT01](./files/egit_004.png)  
    1. Personal access tokens -  token(classic) を選択する。  
    ![PAT02](./files/egit_005.png)
    ![PAT03](./files/egit_006.png)  
    1. Generate new token -  Generate new token(classic) を選択する。  
    ![PAT04](./files/egit_007.png)  
    1. repo にチェックを入れ、Generate tokenを選択する。  
    ※Note は任意の名称や説明を設定してください。   
    ![PAT05設定01](./files/egit_008.png)
    ![PAT05設定02](./files/egit_009.png)  
    1. 作成したトークンが表示されるので、コピーしてテキストファイルなどに保管する。  
    ※後続作業のeclipseにGitHubソースをインポートする際に使用します。  
    ![PAT06](./files/egit_010.png)  
    1. 作成されたトークンの Configure SSO を選択し、リスト表示された組織アカウント（電通総研）の Authorize を選択する。  
    ![Authorize01](./files/egit_011.png)
    ![Authorize02](./files/egit_012.png)  
    ※組織アカウントのリポジトリにアクセス可能に設定したあと、確認するとボタンが Deauthorize になっている事が確認できる。  
    ![Authorize03](./files/egit_013.png)  
    アクセストークンの準備は以上となります。  
1. ローカルIDEへのGitHubソースの取り込み  
    1. ローカルIDEのeclipseで パースペクティブを開く - Git を選択を選択する。  
    ![GitHubソース取り込み01](./files/egit_014.png)  
    1. Gitリポジトリのペインで Clone a Git repository を選択する。  
    ![GitHubソース取り込み02](./files/egit_015.png)  
    1. GitHubソースにアクセスする設定を入力し、 次へ を選択する。  
    ＜設定内容＞  
    URI：GitHubソースのパス  
    ユーザー：GitHubアカウントのユーザー名  
    パスワード：項番 2-7 で作成したパーソナルアクセストークン  
    ![GitHubソース取り込み03](./files/egit_016.png)  
    1. ブランチ選択で 次へ を選択する。  
    ![GitHubソース取り込み04](./files/egit_017.png)  
    1. ローカル宛先の設定を入力し、 次へ を選択する。    
    ＜設定内容＞  
    ディレクトリー：項番1-2 で設定したGitHubソースの管理ディレクトリ  
    ![GitHubソース取り込み05](./files/egit_018.png)  
    1. eclipseのGitリポジトリーペインに取得したGitHubソースが表示されている事を確認する。  
    ![GitHubソース取り込み06](./files/egit_019.png)  
    1. 取り込み対象のGitHubソースを選択 - 右クリックメニュー - プロジェクトのインポート を選択する。  
    ![GitHubソース取り込み07](./files/egit_020.png)  
    1. 取り込み対象のソースのフォルダーを 選択 - 完了 を選択する。  
    ![GitHubソース取り込み08](./files/egit_021.png)  
    プロジェクトエクスプローラーのペインに対象のソースが表示され、インポートが完了します。  
    ![GitHubソース取り込み09](./files/egit_022.png)  

