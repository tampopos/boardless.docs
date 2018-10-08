# services

[inversify.js](https://github.com/inversify/InversifyJS)によって DI 管理されたオブジェクト。  
ビジネスロジックの実装を行う部分で、非同期ロジックの実装、reducer への dispatch、routing 等が行える。

## 実装例

### service interface

interface と 実体は別ファイルで実装する必要がある

```ts
export interface IHogeService {
  execute: (hoge: string, history: History) => void;
}
```

### service

```ts
@injectable()
export class HogeService implements IHogeService {
  constructor(
    @inject('fetchService') private fetchService: IFetchService,
    @inject('messagesService') private messagesService: IMessagesService,
    @inject('dispatchProvider') private dispatchProvider: IDispatchProvider
  ) {}
  private get dispatch() {
    return this.dispatchProvider.dispatch;
  }
  public execute = async (hoge: string, history: History) => {
    // API呼び出し
    const { newHoge } = await this.fetchService.fetchAsync<{
      newHoge: string;
    }>({
      relativeUrl: Url.executeHoge,
      methodName: 'POST',
      body: { hoge }
    });
    // メッセージを表示
    this.messagesService.appendMessages(({ messages }) => ({
      level: 'info' as 'info',
      text: messages.executedHoge,
      showDuration: 5000
    }));
    // storeの更新
    this.dispatch(hogeActionCreators.set({ hoge: newHoge }));
    // 別ページヘrouting
    history.push(`/${newHoge}`);
  };
}
```
