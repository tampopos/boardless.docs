# 規約

## 命名

- 基本的な[C# のコーディング規則](https://docs.microsoft.com/ja-jp/dotnet/csharp/programming-guide/inside-a-program/coding-conventions)に従う。

## オブジェクトの種類

- Domain Model
  - Service  
    ビジネスロジックを外部に公開するためのオブジェクト。副作用を伴わないロジックや、Command を束ねる
  - Command  
    Service の中で副作用を伴うもの。受けたパラメータに対して処理を実行するだけの実装にする
  - Model  
    境界を超えてパラメータを伝播するためのオブジェクト
- Use Case
  - UseCase
    Query,Service(Command)を束ねたり、オブジェクトの Scope を管理するオブジェクト。
  - Model  
    Interface Adapter との境界を超えてパラメータを伝播するためのオブジェクト
- Interface Adapter
  - Receivers  
    Messaging queue のエントリーポイント
  - Controller  
    Web Api のエントリーポイント
  - Model  
    外界 との境界を超えてパラメータを伝播するためのオブジェクト
- External Interface
  - Repository  
    永続化する Entity に対応した I/O を実装するオブジェクト。
  - Query
    パラメータ に対して必要なデータを取得するためのオブジェクト。

## DI

- 可能な限りコンストラクタ解決を使う
- IDisposable なオブジェクト等、インスタンス生成にオーバーヘッドがある場合、解決する時 Lazy で実装する
- ドメイン層、インフラストラクチャ層以下のオブジェクトはスレッドセーフに実装する
- using ステートメント はドメイン層以下には記述しない

## その他

- try-catch は できるだけ書かない
