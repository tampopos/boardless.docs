# services

[inversify.js](https://github.com/inversify/InversifyJS)によって DI 管理されたオブジェクト。  
ビジネスロジックの実装を行う部分で UseCase と Service が該当する。
このうち UseCase は Component との接続ロジックを記述し reducer への dispatch、routing 等が行える。

## 実装例

### symbols

javascript には interface が存在しないため DI の識別子として利用できない。
このため inversify では symbol を識別子として定義する必要がある

```ts
export const useCaseSymbols = {
  hogeUseCase: createRegisterSymbol<IHogeUseCase>()
};
```

### service interface

interface と 実体は別ファイルで実装する必要がある

#### interfaces/hoge-service.ts

```ts
export interface IHogeService {
  execute: (hoge: string, history: History) => void;
}
```

### service

#### hoge-service.ts

```ts
@injectable()
export class HogeUseCase implements IHogeUseCase {
  constructor(
    @inject(symbols.hogeService) private hogeService: IHogeService,
    @inject(symbols.messagesService) private messagesService: IMessagesUseCase,
    @inject(symbols.dispatchProvider)
    private dispatchProvider: IDispatchProvider
  ) {}
  private get dispatch() {
    return this.dispatchProvider.dispatch;
  }
  public execute = async (hoge: string, history: History) => {
    // API呼び出し
    const { newHoge } = await this.hogeService.fetch(hoge);
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

### register

DI Container への登録

```ts
export const registerUseCases = (container: Container) => {
  container.register(useCaseSymbols.hogeUseCase, binder =>
    binder.to(HogeUseCase).inSingletonScope()
  );
};
```

### 呼び出し側

UseCase は 基本的に mapDispatchToProps メソッドで呼ばれることを想定する

#### hoge.tsx

```ts
const mapDispatchToProps: DispatchMapper<Events, OwnProps> = dispatch => {
  const { execute } = resolve(useCaseSymbols.hogeUseCase);
  return { execute };
};
```
