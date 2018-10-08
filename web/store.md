# store 管理

## 概要

store 管理は redux 及び typescript-fsa を使って行う。
データのフローは state → component → actionCreator → reducer → state となる。
ここでは component 以外の実装例を示す

## 実装例

### state

domain 毎に子 state を作り、アプリケーション全体で管理する状態として定義される storedState に追加する。

#### hoge-state.ts

```ts
// パターン1
export const defaultHogeState={
    hoge:string;
}
export type HogeState = typeof defaultHogeState;

// パターン2
export interface HogeState{
    hoge:string;
}
export const defaultHogeState:HogeState={
    hoge:string;
}
```

#### stored-state.ts

```ts
export const defaultState = {
  hogeState: defaultHogeState
};
```

### actionCreators

状態変化を reducer に伝えるための action を作る部分

#### hoge-action-creators.ts

````ts
// typescript-fsa による action-creator を生成するためのfactory
const factory = actionCreatorFactory();
export const hogeActionCreators = {
  // reducerはここで指定する文字列をキーに動作するため、reducer毎に一意に実装する必要がある
  set: factory<{hoge:string}>('hogeActionCreators.set'),
};```
````

### reducer

state を変更する部分。ここにはビジネスロジックは書かない。  
reducer は変更する子 state 毎に定義し、observe する action に対して case を書く。  
このことから action と case は 1 対多の関係性になる。  
最後に createReducers に実装した子 reducer を登録する。

#### hoge-reducer.ts

```ts
export const hogeReducer = (storedState: StoredState) =>
  reducerWithInitialState(storedState.hogeState).case(
    hogeActionCreators.set,
    (s, { hoge }) => {
      // 新規オブジェクトとしてstateを戻すとコミットされる
      return { ...s, hoge };
    }
  );
```

#### reducer-factory.ts

```ts
const createReducers = (initialState: StoredState) =>
  combineReducers<StoredState>({
    hogeState: hogeReducer(initialState)
  });
```

## ローカルストレージ

ローカルストレージに保存する項目を子 state 毎に選択できる。  
※ パフォーマンスに大きく関わるため実装は最小限にすること。

## 非同期

redux のデータフローでは非同期の実装は取り扱えない。これをを解決するためのライブラリとして redux-thunk や redux-saga があるが、これらは使用しない。  
非同期処理は mapDispatchToProps および Services に実装する

- ビジネスロジックの実装は Service に実装して redux と切り分けておきたい

## 関連項目

- [components](.components.md)  
  React.Component と redux を接続するための方法
- [getters](.getters.md)  
  state を拡張するためのメソッド郡を実装する
- [services](.services.md)  
  ビジネスロジックを記述する
