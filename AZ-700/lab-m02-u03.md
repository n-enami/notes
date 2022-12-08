# ラボ (ハンズオン)

手順書: [M02-ユニット 3 仮想ネットワーク ゲートウェイを作成および構成する](https://github.com/MicrosoftLearning/AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions.ja-jp/blob/main/Instructions/Exercises/M02-Unit%203%20Create%20and%20configure%20a%20virtual%20network%20gateway.md)

概要: 2つのVNetをVPNで接続します。

```
CoreServicesVnet
└仮想ネットワークゲートウェイ
                            |
ManufacturingVnet           |VPN
└仮想ネットワークゲートウェイ
```

時間: 70 分 (最大 45 分のデプロイ待機時間を含む)

はじめにお読みください: [全般的なラボの注意](lab.md)

このラボの注意:
- リソースのデプロイに時間がかかります。
- タスク6: 仮想ネットワークゲートウェイの作成には45分ほどかかります。リソースの作成が始まったら、それが完了するのを待たず、タスク7に進んでください。
- タスク7: もう一つの仮想ネットワークゲートウェイを作成します。45分ほどかかります。
- タスク8: タスク6, 7のリソース作成が終わってから、タスク8を開始できます。
- 次のラボでクリーンナップ（リソース削除）を行います。