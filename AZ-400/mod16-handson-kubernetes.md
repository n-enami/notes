# ハンズオン（後編） Kubernetes

■ハンズオンの概要

- 前編
  - 作業用環境として labvm (Windows VM) を準備します
  - Docker実行のため、dockervm（Linux VM）を準備します
  - .NET で、Webアプリを作成します
  - WebアプリをDockerコンテナー化します
- 後編
  - Azure Reposに、開発したコード一式を格納します
  - Azure Container RegistryとAzure Kubernetes Serviceを準備します
  - Azure Pipelinesで、次のパイプラインを作成します
    - Dockerイメージをビルド
    - Azure Container Registryにイメージをプッシュ
    - イメージをAzure Kubernetes Serviceにデプロイ
  - Azure Kubernetes ServicesでWebアプリの動作を確認します

■Gitリポジトリの初期化

- 以下のコマンドを投入
  ```
  dotnet new gitignore
  git init
  git add -A
  git config --global user.name 'test'
  git config --global user.email 'test@example.com'
  git commit -m 'initial commit'
  git branch --move master main
  ```

■新しいプロジェクトを作成

- Webブラウザを開く
- Azure DevOpsの画面に移動 https://dev.azure.com
- `+ New Project`
- Project name: `dockerwebapp1`
- Visibility: `Private`
- Create

■Git用のPersonal Access Tokenの取得

- 画面右上の、右から2番めのアイコンをクリック
- Personal Access Tokens
- `+ New Token`
- Name: gitpassword2 （など、適当に）
- Codeの`Full`にチェック
- Create
- 表示されたTokenをコピーしておく。後で使うのでメモ帳等にコピー

■Azure ReposにコードをPushする

- プロジェクトのReposに移動
- 「Push an existing repository from command line」の下のテキストボックス内のコマンドをコピー
- Visual Studio Codeに切り替える
- Terminalに、コピーしたコマンドを投入
- パスワードの入力が求められるので、先程コピーしたTokenを貼り付ける
- ReposのFilesをクリック
- Files（ファイル一覧）に、`Program.cs`等のファイルが入ってきたことを確認

■Azure Container Registry (ACR) と Azure Kubernetes Service (AKS) の準備

- Cloud Shellを起動
  - https://shell.azure.com にアクセスする
  - Bash / PowerShellどちらでもよい
- 以下のコマンドを投入。
  ```
  az provider register --namespace Microsoft.ContainerRegistry # acr
  az provider register --namespace Microsoft.ContainerService # aks
  az provider register --namespace Microsoft.OperationsManagement # monitoring
  az group create --name hello_rg --location japaneast
  ```
- リソースプロバイダーの登録（上記）が実際に終わるまで少し時間がかかるので3分ほど待つ
- 以下のコマンドを投入。※12341234（2箇所）は、適当な乱数に置き換える
  ```
  az acr create \
    --name myContainerRegistry12341234 \
    --resource-group hello_rg \
    --sku Basic
  az aks create \
    --resource-group hello_rg \
    --name myapp12341234 \
    --node-count 1 \
    --enable-addons monitoring \
    --generate-ssh-keys
  ```

■Azure Pipelinesの準備

- Azure DevOpsの画面に移動 https://dev.azure.com
- Pipelinesをクリック
- Create Pipeline
- Where is your code? → Azure Repos Git
- Select a repository →  `dockerwebapp1`
- Configure your pipeline: Deploy to Azure Kubernetes Service
- サブスクリプションの選択: Azure Pass - スポンサー プラン
- Continue
- 別ウィンドウで認証が求められるので、トレーニング開始時に作成したMicrosoftアカウントでサインイン
- Cluster: 選択肢から選択できるクラスター(myapp～～)
- Namespace: New, `hello`
- Container Registry: 選択肢から選択できるレジストリ(myapp～～)
- Image Name: hello
- Service Port: 8080
- Validate and configure
- (パイプラインのYAMLが生成される)
- Save and run
- 再度 Save and run
- BuildとDeployのステージが表示され、ジョブ実行が進行する。
  - `This pipeline needs permissin to access a resource ...` と表示された場合は、View, Permit, Permit とクリック
- Deployステージが正常に完了（緑の丸にチェック）したらOK


■Webアプリへのアクセス

- 画面左Pipelines以下のEnvironments をクリック
- helloをクリック
- Resourcesで、「hello」（青いKubernetesアイコン）をクリック
- Servicesをクリック
- External IP をコピー
- 新しいWebブラウザタブを開き、アドレス欄に、「External IP:8080」と「:8080」を付けてアクセス
- `Hello World!` と表示されればOK

■ハンズオンの確認

- 開発環境として labvm (Windows VM) を準備しました
- Docker実行のため、dockervm（Linux VM）を準備しました
- .NET で、Webアプリを作成しました
- WebアプリをDockerコンテナー化しました
- Azure Reposに、開発したコード一式を格納しました
- Azure Container RegistryとAzure Kubernetes Serviceを準備しました
- Azure Pipelinesで、次のパイプラインを作成しました
  - Dockerイメージをビルド
  - Azure Container Registryにイメージをプッシュ
  - イメージをAzure Kubernetes Serviceにデプロイ
- Azure Kubernetes ServicesでWebアプリの動作を確認しました

■クリーンナップ（不要なリソースの削除）

- Azure portalに移動
- リソースグループ一覧
- 作成されたリソースグループを削除

以上です。おつかれさまでした！