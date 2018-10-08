# アプリケーション設定

## 読み込み

書き設定を読み込んでパラメータを作成する。  
同じキー項目が存在する場合、上位のルールが採用される。  
json ファイルは存在しない場合無視される。

- command parameter  
  起動時のコマンドパラメータで下記のように指定できる。
  コロンで区切ると入れ子のパラメータになる
  ```bash
  dotnet xxx.dll hoge1=huga1 hoge2=huga2 hoge3:child=huga3
  ```
- 環境変数
- UserSecrets  
  dotnet コマンドでアセンブリごとに指定できる。
  詳細は[こちら](https://docs.microsoft.com/ja-jp/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows)
- `appsettings.user.json`  
  .gitignore でソース管理から除外される設定
- `appsettings.{hostingEnvironment.EnvironmentName}.json`
  Web のみ指定可能。
- `appsettings.json`

## パラメータにマップするクラス

- [web](https://github.com/wakuwaku3/tmpps.boardless.api/tree/master/Tmpps.Boardless.Web/Configuration/BoardlessWebConfig.cs)
- [cli](https://github.com/wakuwaku3/tmpps.boardless.api/tree/master/Tmpps.Boardless.Domain.Common/Configuration/BoardlessConfig.cs)
- [subscriber](https://github.com/wakuwaku3/tmpps.boardless.api/tree/master/Tmpps.Boardless.Domain.Common/Configuration/BoardlessConfig.cs)
