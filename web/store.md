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
// パターン1(スニペット`stated`で展開)
export const defaultHogeState={
    hoge:string;
}
export type HogeState = typeof defaultHogeState;

// パターン2(スニペット`state`で展開)
export interface HogeState{
    hoge:string;
}
export const defaultHogeState:HogeState={
    hoge:string;
}
export default HogeState;
```

#### stored-state.ts

```ts
export const defaultState = {
  hoge: defaultHogeState
};
```

### action

dispatch(store に変更を適用すること) する際の action の payload の型を定義する。
スニペット`action`で展開可能

#### hoge/action.ts

```ts
export interface Action {
  reset: void;
  set: { hoge: string };
}
export default Action;
```

### actionCreators

状態変化を reducer に伝えるための変数を作る部分
スニペット`actc`で展開可能

#### hoge/action-creators.ts

```ts
const actionCreators = createActionCreators('hoge')<Action>('reset', 'set');
export const { reset, set } = actionCreators;
export default actionCreators;
```

### functions

state を変更するための値の整形ロジックを定義する。ここにはビジネスロジックは書かない。  
スニペット`fncs`で展開可能

#### hoge/functions.ts

```ts
const functions: ReducerFunctions<State, Action> = {
  reset: s => {
    return defaultState;
  },
  set: (s, { hoge }) => {
    // 新規オブジェクトとしてstateを戻すとコミットされる
    return { ...s, { hoge } };
  }
};
export default functions;
```

### reducer

actionCreators と functions から reducer を生成する。
スニペット`reducers`で展開可能

#### hoge/index.ts

```ts
import { createReducers } from '../redux-helper';
import actionCreators from './action-creators';
import functions from './functions';

export const hogeReducer = createReducers(actionCreators, functions);
```

#### reducer-factory.ts

```ts
const builders: ReducerBuilders<StoredState> = {
  hoge: hogeReducer
};
```

## ローカルストレージ

ローカルストレージに保存する項目を子 state 毎に選択できる。  
※ パフォーマンスに大きく関わるため実装は最小限にすること。

## 非同期

redux のデータフローでは非同期の実装は取り扱えない。これをを解決するためのライブラリとして redux-thunk や redux-saga があるが、これらは使用しない。  
非同期処理は mapDispatchToProps および Services に実装する

- ビジネスロジックの実装は Service に実装して redux と切り分けておきたい

## 関連項目

- [components](./components.md)  
  React.Component と redux を接続するための方法
- [selectors](./selectors.md)  
  state を拡張するためのメソッド郡を実装する
- [services](./services.md)  
  ビジネスロジックを記述する
