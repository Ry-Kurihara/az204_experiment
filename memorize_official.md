- **App Service**

  - ターゲットフォルダのクリーンアップ

    - az webapp deploy  - clean true

  - zipファイルのロック（本番の読み取り動作などとの衝突）

    - スワップを有効にしてステージングに対してaz webapp deployで解決
      - 本番ワーカーが直接ファイルを触っているところに上書きする機会がなくなり、ロック衝突の可能性が大幅に下がる

  - TLS/SSL証明書

    - App Service証明書の購入

  - Microsoft Graph APIへの権限を与える

    - **なぜ管理IDではなかったのか？？**

    - **シングル テナント**: 自分のテナント内でのみアクセス可能

    - **マルチテナント**: 他のテナントでアクセス可能

    - **アプリケーション** - この種類のサービス プリンシパルは、単一のテナントまたはディレクトリ内のグローバル アプリケーション オブジェクトのローカル表現、つまりアプリケーション インスタンスです。 サービス プリンシパルは、アプリケーションが使用される各テナント内に作成され、グローバルに一意なアプリ オブジェクトを参照します。 サービス プリンシパル オブジェクトには、特定のテナント内でアプリが実際に実行できること、アプリにアクセスできるユーザー、アプリからアクセスできるリソースを定義します。

    - **マネージド ID** - この種類のサービス プリンシパルは、[マネージド ID](https://learn.microsoft.com/ja-jp/azure/active-directory/managed-identities-azure-resources/overview) を表す目的で使用されます。 マネージド ID は、Microsoft Entra 認証をサポートするリソースに接続するときに使用する ID をアプリケーションに提供します。 マネージド ID が有効になっている場合、そのマネージド ID を表すサービス プリンシパルがテナントに作成されます。 マネージド ID を表すサービス プリンシパルには、権利やアクセス許可を付与できますが、直接更新したり変更を加えたりすることはできません。

    - **レガシ** - この種類のサービス プリンシパルは、レガシ アプリを表します。これは、アプリの登録前に作成されたアプリ、またはレガシ エクスペリエンスを使用して作成されたアプリです。 レガシ サービス プリンシパルには以下を含めることができます。

      - 資格情報

      - サービス プリンシパル名

      - 応答 URL

      - 許可されているユーザーが編集できるが、アプリの登録が関連付けられていないその他のプロパティ。

      - ```mermaid
        graph TD
          subgraph テナント
            direction TB
            Tenant1["Microsoft Entra ID<br>テナント (tenant1)"]
            AppObj["アプリケーション オブジェクト<br>(app1)"]
            AppSP["サービス プリンシパル<br>(app1 の SP)"]
          end
        
          WebApp["Azure App Service Web アプリ<br>(app1)"]
          MI["マネージド ID<br>(Managed Identity)"]
          Legacy["レガシ SP<br>(手動 SP)"]
        
          WebApp -->|登録| AppObj
          AppObj -->|テンプレート| AppSP
          AppSP --> MI
          AppSP --> Legacy
        
          %% Microsoft Graph API へ権限付与対象
          Graph["Microsoft Graph API"]
          AppSP -.->|アクセス許可を付与| Graph
        ```

- **ACR**

  - az acr repository delete --name devregistry --image dev/nginx:latest

- **ACI**

  - az container up --source .: 起動する

- **Azure Function**

  - Microsoft Defender for Cloud
    - 未解決のDNSドメイン
      - 関数が増えてくると起こるトラブルみたいなものらしい
    - Standardからサポート
  - トリガーとバインド設定
    - C#クラスライブラリ： C#属性での装飾定義
    - Java: Java注釈での定義
    - スクリプト系：function.json
  - maxConcurrentRequests
  - maxOutstandingRequests
    - 未処理の許容数、キューで待ってあげる：従量課金性だと制限200

- **Strage Account**

  - SASの作成時のベストプラクティス
    - アカウントキーからの発行よりもEntraID資格情報での発行がセキュア

  - C#コードセグメント
    - Recource = b or c
    - b: オブジェクト単体での単位
    - c: コンテナ単位

  - ライフサイクル管理ポリシーのJSON管理

    - prefixmatch：コンテナ名を先頭につける必要あり
    - blockBlob: 一般的なファイルストレージ用のBlob

  - | タイプ          | 用途                                           | 特徴                                                         |
    | --------------- | ---------------------------------------------- | ------------------------------------------------------------ |
    | **Block Blob**  | 画像、動画、ドキュメントなどの一般的なファイル | - データを「最大 100 MiB（1 ブロック）ずつアップロード」 - 大きなファイルは複数ブロックに分割して同時並行アップロードが可能 - 最大容量は約 190.7 TiB |
    | **Page Blob**   | 仮想マシンの VHD（ディスクイメージ）           | - 512 バイト単位の“ページ”に対してランダム読み書き可能 - 高速な I/O が必要なシナリオ向け |
    | **Append Blob** | ログや監査トレイルなど、追記型ワークロード     | - 末尾への追加（Append）専用 - 既存データの上書きは不可      |

  - Que Storage

    - PeekMessage: メッセージの存在確認

- **Azure SQL Database**

  - OLTP: OnLine Transaction Process
    - 可用性が高い。小規模なトランザクションが処理される。
  - OLAP: OnLine Analytics Process
    - 大規模なトランザクション：Azure Analysis Services

- **Azure App Configuration**

  - 全然わからん

- Azure Key Vault

  - | 操作                                     | コマンド例                                                   |
    | ---------------------------------------- | ------------------------------------------------------------ |
    | キーの作成                               | `az keyvault key create --vault-name <VaultName> --name <KeyName> --protection software` |
    | キーの詳細取得（show）                   | `az keyvault key show --vault-name <VaultName> --name <KeyName>` |
    | キーの一覧取得（list）                   | `az keyvault key list --vault-name <VaultName>`              |
    | キー属性の設定（Rotation Policy）        | `az keyvault key set-attributes --vault-name <VaultName> --name <KeyName> --policy path/to/policy.json` |
    | キーのローテーション（新バージョン生成） | `az keyvault key rotate --vault-name <VaultName> --name <KeyName>` |

- Application Insights

  - Webテストとアラート：ユーザー操作をシミュレートする「可用性テスト」を作成
  - サンプリング：テレメトリが多い場合に有効だが正確ではない
  - 標準メトリック：
    - Application Insights がデフォルトで自動収集し、**収集時点であらかじめ集計（pre-aggregate）**して保存するメトリックです
    - よく使う指標が１分間隔でまとめられており、**ダッシュボード表示**や**アラート設定**に最適化されています 

- HTTP リクエストの基本

  - POST: リソースの下に積み上げていくイメージ
    - ただ、実は Blob Storage の REST API ではほとんど使われません。代わりに上記の PUT／GET／DELETE が中心です。
  - PUT: “リソースを完全に置き換える” 操作

- API Management API

  - コードセグメント

    - backend: リクエストフィルター

    - outbound: レスポンスフィルター

    - base: 上位スコープのポリシーの実行

    - ```xml
      <policies>
          <inbound>
              <cross-domain />
              <base />
              <find-and-replace from="xyz" to="abc" />
          </inbound>
      </policies>
      ```

- EventHub

  - az eventhubs eventhub update: パーティションの追加
  - なんかよくわからんコード
    - EventPosition startingPosition = EventPosition.Earliest;
    - string partitionId = (await consumer.GetPartitionIdsAsync()).First();
  - Azure EventHub Captureで配信できる先
    - Azure Blob
    - Data Lake Storage Gen1, Gen2

- EventGrid

  - フィルタできる条件は3カテゴリ
    - イベントの種類
    - 件名の始まりまたは終り
    - 高度なフィールドおよび演算子（高度なフィルター処理：Advanced）
  - 指定された再試行スケジュール内で配信されないイベントをキャプチャするために [配信不能] を有効にします。

- Service Bus

  - 配信不能こちらもある

  - セッション機能：FIFOの担保

  - | パターン                     | ユースケース例                                               | メリット                                                     | デメリット                                                   |
    | ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | **単純要求／応答**           | クライアントがキューにリクエストを送り、ワーカーが処理後に専用のレスポンス キューへ返信 | - 実装がシンプル- トレーサビリティ（MessageId⇔CorrelationId）が取りやすい | - 1:1 通信のみ- スケールはリクエスター／レスポンダー双方に依存 |
    | **マルチキャスト要求／応答** | 発行元がトピックへメッセージをパブリッシュし、複数のサブスクライバーが同じリクエストを処理＆各自が応答を返す場合 | - 複数コンシューマーで並列／独立処理可能- サブスクライバーを動的に増やせる | - 応答の集約や失敗ハンドリングが複雑- トピックのスケーリングコスト が増大 |
    | **多重化（セッション）**     | SessionId で関連メッセージ群を識別し、１つの受信プロセスが同一セッション内を順序どおり処理 | - メッセージ順序保証- 関連メッセージの単一コンシューマー割り当て- レースコンディション排除 | - セッション単位でロック／リソースを確保- セッションタイムアウト・死活管理が必要 |
    | **多重化された要求／応答**   | 複数の発行元が同一応答キューを共有し、ReplyToSessionId で自分宛のセッションを指定して返答を受け取る | - 発行元ごとに応答先を分離できる- 双方向通信をスケーラブルに実現 | - ReplyToSessionId の管理・復元が煩雑- 応答セッションが残存するとキュー保持コストがかさむ可能性あり |
    
    トピックのフィルター条件で設定可能なもの
    
    - SQL, bool, 相関関係