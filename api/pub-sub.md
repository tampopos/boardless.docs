# Google Cloud Pub/Sub

## 設定方法

#### 認証情報

Google Cloud Platform 環境外の場合のみ設定を行う必要がある。  
ダウンロード した認証用 JSON のパスを環境変数に設定する。

```bash
echo 'export GOOGLE_APPLICATION_CREDENTIALS="$full_path"'
source ~/.bashrc
```

詳細は下記。  
https://cloud.google.com/pubsub/docs/reference/libraries?hl=ja

#### アプリケーションとの紐付

[設定](./appsettings.md)に ProjectId,TopicId,SubscriptionId を指定する。
