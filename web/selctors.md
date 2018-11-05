# selectors

store 管理で管理する state はシリアライズ可能であることが望ましい。  
このため mapStateToProps 等で呼び出す state に依存する get メソッドを共通化したい場合は selectors オブジェクトに定義する。
スニペット`selectors`で展開可能。

## 実装例

### hoge-selectors.ts

```ts
export class HogeSelectors {
  constructor(private state: State) {}
  public get existsHoge() {
    const { hoge } = this.state;
    return Boolean(hoge);
  }
}
```

### 呼び出し側

```tsx
const mapStateToProps: StateMapperWithRouter<Props, Param, OwnProps> = ({
  hogeState
}) => {
  const { existsHoge } = new HogeSelectors(hogeState);
  return { existsHoge };
};
```
