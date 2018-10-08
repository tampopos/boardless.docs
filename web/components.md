# components

ビジネスロジックに依存する React.Component は pages 配下に実装する。
ビジネスロジックに依存しない React.Component は components 配下に実装する。

## 実装例

コンポーネント内で state 管理やライフサイクルフックが不要な場合 SFC(Stateless Functional Component)として、必要な場合 ComponentClass として実装する。

#### hoge.tsx

```tsx
// スニペット(ssfc|sccc)で以下のテンプレートが実装される

// Component Scope の css を定義する
const styles = (theme: Theme) =>
  createStyles({
    root: {}
  });

// コンポーネント内でアクセスする関数以外の props を定義する
interface Props {
  resources: Resources;
  history: History;
}
// url から 取得するパラメータを定義する(optional)
interface Param {}
// コンポーネント外部に公開する props を定義する(optional)
interface OwnProps {}
// コンポーネントに注入する state をマッピングするメソッド。
const mapStateToProps: StateMapperWithRouter<Props, Param, OwnProps> = (
  // StoredState
  { accountsState },
  // RouteComponent<Param> & OwnProps
  { history }
) => {
  // State のみに依存する共通メソッドは Getters に実装する
  const { resources } = new AccountsGetters(accountsState);
  // Partial<Props> を戻す
  return { resources, history };
};
// コンポーネントから store にアクセスする関数を定義する
// Eventsはコンポーネントの props にマップされる(optional)
interface Events {}
// コンポーネントから store にアクセスする関数を実装する(optional)
const mapDispatchToProps: DispatchMapper<Events, OwnProps> = dispatch => {
  // 複雑なロジックはServiceに切り出す
  return {};
};
// スニペット(ssfc)で展開した場合
// StyledSFC型で定義する
const Inner: StyledSFC<typeof styles, Props & Events> = props => {
  // 変数は出来る限り展開してアクセスする
  const { resources } = props;
  // Componentの外側で注入した css と styles をまとめる
  const { root } = getInjectClasses(props);
  return (
    <Container className={root}>
      <Row>
        <div>{resources.AddingWorkspace}</div>
      </Row>
    </Container>
  );
};
// スニペット(sccc)で展開した場合
// Class 内で管理する state を定義する
interface State {}
class Inner extends StyledComponentBase<typeof styles, Props & Events, State> {
  constructor(props: any) {
    super(props);
    this.state = {};
  }
  public render() {
    // 変数は出来る限り展開してアクセスする
    const { resources } = this.props;
    const { root } = getInjectClasses(this.props);
    return (
      <Container className={root}>
        <Row>
          <div>{resources.AddingWorkspace}</div>
        </Row>
      </Container>
    );
  }
}
// コンポーネントに css を 注入する
const StyledInner = decorate(styles)(Inner);
// コンポーネントに store と router を接続する
// 接続しない場合 StyledInner をそのまま公開する
export const Hoge = withConnectedRouter(mapStateToProps, mapDispatchToProps)(
  StyledInner
);
```

## 関連項目

- [styles](./styles.md)  
  css の実装方法
- [getters](./getters.md)  
  mapStateToProps での state の拡張
- [services](./services.md)  
  mapDispatchToProps 内でのビジネスロジックの呼び出し
- [local](./local.md)  
  多言語対応
