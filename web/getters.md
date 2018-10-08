# getters

store 管理で管理する state はシリアライズ可能であることが望ましい。  
このため mapStateToProps 等で呼び出す state に依存する get メソッドを共通化したい場合は getter オブジェクトに定義する。

## 実装例

### hoge-getters.ts

```ts
export class HogeGetters {
  constructor(private state: HogeState) {}
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
  const { existsHoge } = new HogeGetters(hogeState);
  return { existsHoge };
};
```
