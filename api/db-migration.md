# データ移行

## 実行

各アプリケーション実行時に、接続 DB を自動的にマイグレーションする。
手動実行する場合、`npm run migration`を実行する。

## ルール

- `/.migration`配下の`.sql`ファイルをファイル名昇順で実行する。
- ファイル名をキー に sql の実行状況がテーブルに記憶される。
  - 一度実行した SQL は修正しても再実行されない。(event sourcing)
- Docker を利用している場合、`/boardless/postgresql`にデータが格納される。
