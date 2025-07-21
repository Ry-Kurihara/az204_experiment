## IIS

- Internet Information Services
  - Settings -> Configurationから設定できる

## Domain

- セキュリティ証明書
- デフォルトドメインであればmicrosoftが証明書を作ってくれているのでHTTPS通信もできる
  - カスタムドメインの場合は自分で作ってアップロードしたりする：PFXファイル
    - IP based SSL
    - SNI SSL
      - いろんなブラウザ対応

## CosmosDB

- PartitionKey

## SQL Database

- Always Encrypted?
- Server level and database level
- When use customer Managed Key, we can use Vault



## Application Insight

- It is extension of Azure Monitor
  - if you enable it, you can monior som performances on Azure Monitor.
- SDK is enabled. 
  - これ問題に出てた外部APIの何か？？

## 個別項目調査

- CosmosDB Changed Feed
  - 検知されず困ってたやつこれか