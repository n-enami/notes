# Azure Pipelinesのエージェント

## エージェントとは

エージェントは、一度に1つのジョブを実行する、エージェントソフトウェアを使用するコンピューティングインフラストラクチャ。

要するにWindows/Linux/Macなどのマシン。VMでもよいし、Mac Miniなどの、オンプレミスのコンピュータでもよい。

Windows/Linuxの場合は、更にそのマシン（エージェント）上でDockerコンテナーを起動し、そこでジョブを実行することもできる。

ジョブを実行するためには[エージェント](https://docs.microsoft.com/ja-jp/azure/devops/pipelines/agents/agents?view=azure-devops&tabs=browser)が必要。

いわゆる「ワーカー」マシン。[Jenkinsでいう、ジョブを実行する「ノード」](https://knowledge.sakura.ad.jp/5433/)に相当。

## Microsoft ホステッドとセルフホステッド エージェント

エージェントの種類は2種類。
- Microsoft によってホストされるエージェント(Microsoft-hosted agents)
- セルフホステッド エージェント(Self-hosted agents)

2種類のエージェントの違い。

- Microsoft によってホストされるエージェント(Microsoft-hosted agents)
  - メンテナンスとアップグレードが自動的に行われます。
  - パイプラインを実行するたびに、パイプライン内の各ジョブに対して新しい仮想マシンが作成されます。
  - 仮想マシンは、1つのジョブの後に**破棄される**。
    - これはメリットでもありデメリットでもある。
      - メリット: 環境が毎回リセットされるので、以前のビルドの影響がなく、トラブルが起きにくい。ステートレス。
      - デメリット: 同じビルドを実行する際に以前のビルドで利用した環境などのキャッシュを利用できない。
  - ジョブを実行する最も簡単な方法
  - ジョブの時間制限あり
- セルフホステッド エージェント(Self-hosted agents)
  - ジョブを実行するために独自に設定および管理するエージェント
  - Linux、macOS、Windows マシンに、エージェントのソフトウェアをインストールする
  - Dockerエージェントも利用できる（コンテナーをエージェント化できる）
  - ビルドとデプロイに必要な依存ソフトウェアをインストールするためのより詳細な制御が可能になる
  - マシン レベルのキャッシュと構成は実行から実行まで保持され、速度が向上する可能性がある
  - 1つのマシンに複数のエージェントをインストールすることもできる（推奨されない）
  - ジョブの時間制限なし