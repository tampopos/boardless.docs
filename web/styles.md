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
  // getInjectClasses で classes を取得する
  const { root,child } = getInjectClasses(props);
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
        <Child injectClasses={{root:child,child}}/>
      </div>
    );
});
```

## 関連項目

- [jss 公式ドキュメント](https://github.com/cssinjs/jss)
