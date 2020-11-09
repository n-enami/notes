# 貼り付け

Cloud Shellで貼り付けを行う場合は、`Ctrl + Shift + V` を使用してください。

複数行のコマンドをコピー・ペーストして実行する際、最終行が貼り付けられただけで実行されない場合があります。貼付け後、Enterキーを押して、最終行も実行されるようにしてください。

# Cloud Shell 環境へ、ラボのファイルをコピーする

Azure portalでCloud Shell （Bash）を起動し、下記のコマンドを貼り付けます。

```
git clone https://github.com/MicrosoftLearning/AZ-104JA-MicrosoftAzureAdministrator.git
cp `find AZ-104JA-MicrosoftAzureAdministrator/Allfiles/Labs -type f` .
```

# Cloud Shell でのファイルの編集について

ファイルを編集する必要がある場合は、Cloud Shellの組み込みのエディターを使用できます。

「{}」ボタン（エディターを開く）で、エディターを起動できます。

エディターを閉じる際は、Cloud Shell右上の「✕」ではなく、「...」アイコンから「エディターを閉じる」、または `Ctrl + Q`で閉じてください。

# ラボ1

タスク2の7でメンバーシップの種類の選択のところがグレーアウトして選択できない場合は、Webブラウザでページをリロードしてください。

最後の「リソースのクリーンアップ」の「8.Azure portal からサインアウトし、サインインし直します。」はなくてもできる場合があります。最初に削除を試してみて、ダメならサインアウト、サインインしてみてください。

# ラボ2a

クリーンアップのコマンドでは、
'[id]' ではなく 'id' と書いてください。

# ラボ2b

# ラボ3a

演習 1
タスク 1: リソース グループを作成し、リソース グループにリソースをデプロイする

3 リージョン このラボで使用するサブスクリプションで使用できる Azure リージョンの名前
westus2（米国西部2）を使ってください。

# ラボ3b

演習 1
タスク 2: ARM テンプレートを使用して Azure マネージド ディスクを作成する
3
「テンプレートの編集」 ブレードで、「ファイルの読み込み」 をクリックし、前の手順でダウンロードしたテンプレート ファイルをアップロードします。

は自分でダウンロードしたものではなく、\Allfiles\Labs\03\az104-03b-md-template.json および \Allfiles\Labs\03\az104-03b-md-parameters.jsonを使ってください。

タスク 2: ARM テンプレートを使用して Azure マネージド ディスクを作成する

８．「カスタムデプロイ」 ブレードに戻って、次の設定を指定します。
のLocationはeastusになりますが、そのまま進めて大丈夫です。

# ラボ3c

# ラボ3d

3dの最後のコマンドがラボ３で使ったリソースの削除コマンドになっています。よって、3cで終わるとリソースがすべて残ります。終わった方は3dの最後のコマンドでリソースグループすべて削除してください。
