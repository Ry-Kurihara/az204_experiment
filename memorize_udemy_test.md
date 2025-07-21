## Udemy English Sample001

- blob1ファイル上限: 190TB
  - 全体だと: 5PB
- ASEはApp serviceの環境を強化する
- MongoDB: 6189, SQL Server: 1433
- Redis Cache: 53GB（P4 Plan）よりも増やそうと思ったらクラスターモードでシャーディング
- AzCopy: Blobなどへの大量コピー：azcopy copy a a --recursive
- CDN: Contents Delivery Network: 分散ノードによる高速キャッシュ、高速ロード
  - Akamai, Verison：Akamiはモバイル専用機能がない
  - CDNは結構リッチになってきていて、動画画像をモバイルかそうでないかによって解像度を変えたり、リンクを先回りして用意しておいたりする
- Previleged Managed Identity: 特権的ロールを監視
- 整合性モデル
  - 取り合えず、Strong, Session, Eventualを覚えておけば良い
  - Session（同一セッションでStrong, 別セッションでEventual）: 買い物かごの状態などは最新にしたいWebサイト
- at-most-once: 重複は絶対やだ。ミスは許容
- Storage Account: VMからのリクエストLimit: 20,000 Req/Sec
- EventHubストリーミング：EventGridプッシュ
  - ServiceBusもQueで、EventHubの前身と言える
  - Microsoft.ServiceBus.Messagingクラスの中にEvent Hub Clinetというツールがあって、これが独立して今のEvent Hubに
  - ServiceBusとStorageQueはメッセージング機能特化（FIFO,トランザクション）と言う違いがある
  - Evnet Hub: 1スループットユニットで1MB/s, 1000Req/s増える。20まで増やせるらしい。StandardプランのAuto-infrate機能で、負荷によって20まで調整することも可能
- ATP: for your SQL Database
- Azure Function: PHP not supported
- Integration Service Environment for Logic App: LogicAppはノーコードでのアプリ作成ツール
  - App ServiceにおけるASEのような役割
- App Serviceプラン階層：Free, Basic, Standard, Premium
  - Standardからオートスケール機能使える
  - Free: 10, Basic:100, それ以降：無制限
- CosmosDB container capacity: Unlimited
- New-AzWebApp
- ツール群
  - Visual Studio Remote Tool: Azure VMをリモートデバッグ
  - Function Core Tool: Azure Functionをリモートデバッグ
- VMのスケール限界：1000
- Azure SQL Databaseの機能：Elastic Database：データベースを一つのリソースプールに入れて節約。同時アクセスには弱い。
- Azure Container Instance: az container createコマンドで作る
- DNSレコード：カスタムドメイン対応のためにAレコードとCNAMEレコード
  - Aレコード：IPアドレス紐付け、CNAMEレコード：サブドメイン紐付け

## Udemy Japan 001 TestA

- Azure Function
  - Durable Function
    - Azure Functionのオーケストレーションサービス
    - Fan-in,Fan-out: 並列実行
    - Function Chain: 直列実行
    - Monitor Pattern: 外部実行のステータスによるトリガーなど
  - 設定ファイル
    - function.json: トリガーやバインディングの定義
    - 入力バインディング：0以上（任意の数またはゼロ）
  - カスタムハンドラー
    - Go, PHP, Ruby, Rustなど対応できる
- ASP.NET Core ロギングコード
  - Logger.LogCritical("message")
- ASP.NET ロギングコード
  - 診断ログにも出力：System.Diagnostics.Trace.TraceError("message");
  - Console.WriteLine("message")：コンソールのみへの書き込みであり、ファイルシステム/Blobには書き込まれない。これはアプリケーション診断ログには含まれず、ログストリームでのみ確認ができる
- Windows Web App
  - ログの保存先：ファイルシステムとBlob
- Azure File Sync
  - Azure Storage Servicenのファイル共有について、部分的なオンプレ移行に使える（キャッシュ機能がある）
- ACRのURL（youruniquename.azurecr.io形式）
- Blob 
  - LifeCycle: 更新日ベースでポリシーが適用される。アクセスしただけでは更新日は変わらない
- ユーザー割り当て管理ID
  - 複数アプリがKey Vaultアクセスする際のベストプラクティス

- CosmosDB

  - 以下のように、API名称とデータモデルだけを抽出した表にしました。

    | API 名称           | データモデル            |
    | ------------------ | ----------------------- |
    | **Core (SQL)**     | ドキュメント (JSON)     |
    | **MongoDB API**    | ドキュメント (BSON)     |
    | **PostgreSQL API** | リレーショナル (表／行) |
    | **Cassandra API**  | カラムファミリ          |
    | **Gremlin API**    | グラフ                  |
    | **Table API**      | キー・バリュー          |



## Udemy Japan 002 TestB

- Linux App Serviceではログ出力がファイルシステムのみなことに注意

  - Windows版では、IIS: Internet Information Service（Nginxと同じカテゴリ）がベースとなってログの収集を行なっているため、Blobなどへの出力が可能だったが、Linux基盤のホストではIISが動かせないからだと考えられる。

- App Serviceの裏方エンジン：Kudu

  - 管理コンソールの提供
    - {your-site}.scm.azurewebsites.netで環境変数の確認ができる
    - サブリージョン識別子（スタンプ）がある場合：scm.sub_region.azurewebsites.net
      - App Serviceの混雑回避のための複数のサーバー群の区分
      - 論理的な区分であり、物理的に離れているとは限らない：eastus-001とeastus-002が同じデータセンターにある可能性もある
  - Gitプッシュ、ZIPデプロイなど、デプロイの支援
  - バッチジョブの定期実行支援

- Get-AzVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"

  - EastUS地域における公開されているWindows Server OSイメージのリスト

  - | コマンド                      | 説明                                                         | 使用例                                                       |
    | ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | `Get-AzVMImagePublisher`      | EastUS でイメージを出しているベンダー（Publisher）一覧を取得。例: MicrosoftWindowsServer, Canonical, RedHat,… | `Get-AzVMImagePublisher -Location "EastUS"`                  |
    | `Get-AzVMImageOffer`          | 指定したベンダー（Publisher）が提供する製品群（Offer）一覧を取得。例: WindowsServer, SQLServer-2019-WS2019,… | `Get-AzVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"` |
    | `Get-AzVMImageSku`            | Offer（例: WindowsServer）内のエディション（SKU）一覧を取得。例: 2016-Datacenter, 2019-Datacenter,… | `Get-AzVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"` |
    | `Get-AzResourceGroup`         | サブスクリプション内のリソースグループ一覧を取得             | `Get-AzResourceGroup`                                        |
    | `New-AzResourceGroup`         | 新しくリソースグループを作成。例: MyRG を EastUS に作る      | `New-AzResourceGroup -Name MyRG -Location "EastUS"`          |
    | `Get-AzStorageAccount`        | 指定 RG（例: MyRG）内のストレージアカウント一覧を取得        | `Get-AzStorageAccount -ResourceGroupName MyRG`               |
    | `New-AzStorageContainer`      | Storage Account 内に BLOB コンテナを作成。                   | `New-AzStorageContainer -Name logs -Context $ctx`            |
    | `New-AzFunctionApp`           | 新しい Function App（サーバーレス関数）を作成                | `New-AzFunctionApp -Name MyFunc -ResourceGroupName MyRG -StorageAccountName mystorage -Plan MyPlan` |
    | `New-AzRoleAssignment`        | 指定ユーザー／SP に RBAC ロールを割り当て。例: dashboard-admin に Contributor を付与 | `New-AzRoleAssignment -ObjectId $user.ObjectId -RoleDefinitionName "Contributor" -Scope "/subscriptions/.../resourceGroups/MyRG"` |
    | `New-AzEventGridSubscription` | Event Grid サブスクリプションを作成。例: MyTopic → Function App へイベントを送る | `New-AzEventGridSubscription -ResourceGroupName MyRG -TopicName MyTopic -Endpoint "https://<func>.azurewebsites.net/api/handler"` |

- Azure Function 

  - function.jsonのDirectionプロパティ：in, out, inout
  - だけどpythonなどの場合のv2 Functionだと、host.jsonしかない、そういった設定ファイルは見えないようになっている

- GRS: availability zoneは2つなので2*3=6、9じゃないよ

- ACRのカバー範囲：**Dockerイメージ、OCIイメージ、OCIアーティファクト、Helmチャート**

  - ISOイメージ：光学ディスクの完全コピー

- ACIのFQDN: **mycontainer.(azureregion).azurecontainer.io**

  - ACRと違って、リージョン名が入ってることに注意！
  - コンテナの実行基盤がリージョンベースなので、グローバルでの一意性チェックが面倒だかららしい

## Udemy Japan 003 TestC

- ARM Affinity

  - affinity: 相性

- Kubernetes

  - コントロールプレーン -> ノード
  - Podはあくまでノードの中のさらに1単位

- SAS

  - | SAS 種類            | メリット                                                     | できること                                                   | できないこと                                                 |
    | ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | **ユーザー委任SAS** | - Azure AD 認証（ユーザー／マネージドID）で発行- アカウントキー不要で高セキュリティ | - Blob Storage／Data Lake Storage（`dfs`）へのアクセス- OAuth2 ベースの RBAC 連携 | - Queue／Table／File Storage 非対応- ストアドアクセスポリシー非対応 |
    | **サービスSAS**     | - アカウントキーで即時生成可能- ストアドアクセスポリシー対応でトークン失効管理が容易 | - 単一サービス（Blob・Queue・Table・File）のリソース操作- コンテナ／キュー単位での細かな権限設定（読み書き削除など） | - 複数サービス横断アクセス不可                               |
    | **アカウントSAS**   | - 複数サービス（Blob／Queue／Table／File）を横断的にアクセス可能- サービスレベル API 操作可 | - 上記サービスすべてのリソース操作- `Get/Set Service Properties`、`Get Service Stats` 等のサービス管理操作 | - Azure AD 認証非対応（アカウントキー必須）- ストアドアクセスポリシー非対応 |

- App Serviceログ

  - 情報の下のレベルに詳細、と言うのがあることに注意

  - | メソッド                                       | TraceEventType  | Azure のログ レベル（日本語） | 説明                                 |
    | ---------------------------------------------- | --------------- | ----------------------------- | ------------------------------------ |
    | `Trace.TraceError("…")`                        | Error           | エラー                        | エラー時のみ表示                     |
    | `Trace.TraceWarning("…")`                      | Warning         | 警告                          | 警告以上（警告・エラー）が表示       |
    | `Trace.TraceInformation("…")`                  | Information     | 情報                          | 情報以上（情報・警告・エラー）が表示 |
    | `Trace.WriteLine("…")``Console.WriteLine("…")` | Verbose (Write) | 詳細                          | 詳細レベルのときにのみ表示           |

- App Service言語バージョン

  - 構成 -> 一般設定

- App Service スケールアップ、スケールアウト

  - 料金 = ティア（スケールアップ） * インスタンス数（スケールアウト）

- App Service ファイルシステムへのログ記録

  - 12時間がリミット

- App Service デバッグ機能オン

  - ASPNETCORE_ENVIRONMENT="Development"

- CosmosDB: QueryRequestOptions.ConsistencyLevelで整合性レベルを調整可能

- VMインスタンスファミリー特徴

  - | インスタンスファミリー        | 主な特徴                                                     |
    | ----------------------------- | ------------------------------------------------------------ |
    | **Bシリーズ**                 | バースト可能な低コストインスタンス。CPUクレジットを蓄積／消費。開発環境や小規模Webサーバー向け。 |
    | **Dsv4 (汎用)**               | CPU/メモリのバランス重視。多目的ワークロードに最適。ACU中程度、一時ディスクあり。 |
    | **Fsv2 (コンピュート最適化)** | vCPUあたりのACUが高く、CPU性能に特化。大規模スケール可能。バッチ処理や高負荷Webサーバー向け。 |
    | **Esv4 (メモリ最適化)**       | メモリ/vCPU比が高い。データベースやインメモリキャッシュ、SAP HANAなどのメモリ集約型アプリ向け。 |
    | **Lsv2 (ストレージ最適化)**   | 高速大容量NVMe一時ディスクを搭載。ビッグデータ処理やログ集計、キャッシュ用途に最適。 |
    | **Nシリーズ (GPU搭載)**       | GPUを搭載。AI/機械学習トレーニング・推論やグラフィックスレンダリング向け。 |

- Azure Function

  - if (myTimer.IsPastDue): 関数が遅れて開始した場合
  - **トリガーは関数につき正確に一つ！**

- ARM Template

  - Microsoft.Resources/deployments: 別のARMテンプレートを動かしたい場合

## Udemy Japan 004 TestC

- Redis Cache料金：リージョン、価格帯、時間で決まる

- CosmosDB変更フィード

  - 変更された文書の並べ替えられたリストを、変更が行われた順序で出力する
  - 現状は挿入と更新だけ検知
  - プレビュー機能で現在削除を検知する機能も存在する

- App Configuration:

  - アプリ設定ファイルにハードコーディングされるのではなく、Key Vaultから秘密を取得できるようになる

- Azure Function: コールドスタート問題

  - 使用されていないアプリケーションが起動に時間がかかる現象

- ACIログ

  - 設定 > コンテナ > ログ

- ARMテンプレートエクスポート機能

  - parameters.json, template.jsonがダウンロードされる
  - Dockerfileはダウンロードされない

- EntraID

  - Authenticatorアプリ、テキストメッセージ、電話通話、セキュリティキー

- ACI

  - サーバーレスサービスと位置付けられる：管理されたサーバーレスのAzure環境におけるオンデマンドコンテナ

- ACR

  - az acr config retention update --registry myregistry --status enabled --days 30 --type UntaggedManifests

  - | コマンド                                                     | 説明                                                         |
    | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | `az acr login --name <registry>`                             | ACR にログインし、Docker CLI の `push`/`pull` を有効化       |
    | `az acr build --registry <registry> --image <repo>:<tag> .`  | ローカルまたはリポジトリのソースからイメージをビルドしてプッシュ |
    | `az acr import --name <registry> --source docker.io/library/nginx:latest --image nginx:imported` | Docker Hub など他レジストリからイメージをレジストリ内にインポート |
    | `az acr config retention update --registry <registry> --status enabled --days <日数> --type UntaggedManifests` | 未タグ付けイメージの自動保持(削除)ポリシーを設定             |
    | `az acr webhook create --registry <registry> --name <webhook> --uri <https://…> --actions push` | イメージプッシュ時に指定 URI へ通知する Webhook を作成       |
    | `az acr replication create --registry <registry> --location <リージョン>` | レジストリを指定リージョンへレプリケーションし、地理冗長を構成 |

- Kudu SCM Endpoint

  - Zip API

  - | エンドポイント                                 | 機能                                                         |
    | ---------------------------------------------- | ------------------------------------------------------------ |
    | `GET /api/vfs/{path}/`, `PUT /api/vfs/{path}/` | 仮想ファイルシステムでのディレクトリ一覧取得／ファイルアップロード ([GitHub](https://github.com/projectkudu/kudu/wiki/rest-api?utm_source=chatgpt.com)) |
    | `GET /api/zip/{path}/`, `PUT /api/zip/{path}/` | フォルダーをZIPダウンロード／アップロードして展開 ([GitHub](https://github.com/projectkudu/kudu/wiki/rest-api?utm_source=chatgpt.com)) |
    | `PUT /api/zipdeploy`, `POST /api/zipdeploy`    | ZIP ファイル／URL からのアプリ非同期／同期デプロイ ([GitHub](https://github.com/projectkudu/kudu/wiki/rest-api?utm_source=chatgpt.com)) |
    | `GET /api/environment`                         | Kudu のバージョン情報取得 ([GitHub](https://github.com/projectkudu/kudu/wiki/rest-api?utm_source=chatgpt.com)) |
    | `GET /api/settings`, `POST /api/settings`      | 環境変数一覧取得／新規作成・更新（JSON ボディ指定） ([GitHub](https://github.com/projectkudu/kudu/wiki/rest-api?utm_source=chatgpt.com)) |
    | `POST /api/command`                            | 任意コマンド実行と標準出力取得（JSON ボディ指定） ([GitHub](https://github.com/projectkudu/kudu/wiki/rest-api?utm_source=chatgpt.com)) |

- App Service リージョン

  - App Serviceが実行されるのはApp Serviceプランの属するリージョン
  - このリージョンは変更することができない
  - 変更したい場合、planを新リージョンで作成、このプランにアプリをコピペしたものを新規作成し紐付ける

- Azure Storage Service: Azure Files

  - ネットワークドライブとしてローカルPCzにマウントできる：Windows, Linux, Mac対応

