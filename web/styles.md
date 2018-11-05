# styles

css の実装については [jss](https://github.com/cssinjs/jss) および react-jss を使って行う

## 実装例

style オブジェクトの詳細な実装は[公式ドキュメント](https://github.com/cssinjs/jss)を参照する

## Scoped styles

Component に閉じた CSS を実装する場合

```tsx
const styles = createStyles({
  root: {
    // camel case で定義する。pxは省略可能。
    marginLeft: 10
  }
});
export const Styled = decorate(styles)(props => {
  const { classes } = props;
  const { root } = classes;
  return <div className={root}>xxx</div>;
});
```

## Themed styles

Theme を注入できる

```ts
const styles = (theme: Theme) =>
  createStyles({
    root: {
      marginLeft: theme.shared.margin
    }
  });
```

## Global styles

Global な css として定義できる。
html や body 等の react 外のオブジェクトに反映させたい場合等限定的に使う

```tsx
const styles = createStyles({
  '@global': {
    html: {
      height: '100%'
    }
  }
});
```

## Inject styles

呼び出し側から CSS を注入する

```tsx
const childStyles = createStyles({
  root: {},
  child: {}
});
export const Child = decorate(childStyles)(props => {
  // createPropagationProps で classes を取得する
  const { classes } = createPropagationProps(props);
  const { root,child } = classes;
  // class の合成には appendClassName を使う
  const className = appendClassName(root,child)
  return <div className={className}>xxx</div>;
});
const styles = createStyles({
  parent: {}
  child: {
    marginLeft: 10
  }
});
export const Parent = decorate(styles)(props => {
  const { classes } = props;
  const { parent,child } = classes;
  return (
      <div className={parent}>
        /* rootに注入する場合 */
        <Child className={child}/>
        /* child と root に注入する場合 */
        /* injectClasses は MaterialUI Component の classes に該当するもの */
        <Child injectClasses={{root:child,child}}/>
      </div>
    );
});
```

## 関連項目

- [Material-UI 公式ドキュメント](https://material-ui.com/)
  - [Grid](https://material-ui.com/layout/grid/)  
    Bootstrap like な Grid システムの実装
  - [Breakpoints](https://material-ui.com/layout/breakpoints/)  
    media クエリを書くときに参照する
  - [Transitions](https://material-ui.com/utils/transitions/)  
    animation を付けたい時
  - [Themes](https://material-ui.com/customization/themes/)  
    テーマ に関して
  - [Overrides](https://material-ui.com/customization/overrides/)  
    標準コンポーネントのスタイルの上書き
  - [Default Theme](https://material-ui.com/customization/default-theme/)  
    Materil-UI のテーマの初期値
- [jss 公式ドキュメント](https://github.com/cssinjs/jss)
