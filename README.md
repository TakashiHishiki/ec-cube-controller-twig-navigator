# EC-CUBE Controller Twig Navigator

EC-CUBE Controller Twig Navigator は、EC-CUBE の PHP コントローラーアクションと Twig テンプレートを、VS Code の右クリックメニューから相互に移動できる拡張機能です。

コントローラーから対応する Twig を開く、Twig からそれを描画しているコントローラーアクションへ戻る、といった移動をエディター上で完結できます。

## 主な機能

- コントローラーアクションから関連する Twig テンプレートを開く。
- Twig テンプレートから、そのテンプレートを描画しているコントローラーアクションへ移動する。
- `@Template` アノテーション、PHP 8 Attribute、`render()`、`renderView()` に対応。
- アクションメソッド内だけでなく、関数定義行や直前の `@Template` 記述からもテンプレートを解決する。
- `app/template/<custom>/`、`app/template/default/`、`src/Eccube/Resource/template/default/` を検索対象にする。
- カスタムテンプレートディレクトリを自動検出、またはコマンドパレットから明示的に選択できる。
- 複数候補がある場合は Quick Pick で移動先を選択できる。
- 対応する Twig が存在しない場合、`app/template/default/` 配下に新規作成できる。

## インストール

VS Code の Quick Open を開き、次のコマンドを実行します。

```text
ext install colscenery.ec-cube-controller-twig-navigator
```

または、Visual Studio Marketplace で `EC-CUBE Controller Twig Navigator` を検索してインストールしてください。

## 使い方

### コントローラーから Twig を開く

PHP コントローラーファイルを開き、アクションメソッドの中、または関数定義行の上で右クリックし、`EC-CUBE: Open Related Twig Template` を選択します。

拡張機能は、カーソル位置のアクションから次のような Twig 参照を探します。

```php
/**
 * @Template("Product/detail.twig")
 */
public function detail()
{
    return [];
}
```

```php
#[Template("Product/detail.twig")]
public function detail()
{
    return [];
}
```

```php
public function detail()
{
    return $this->render('Product/detail.twig');
}
```

```php
public function detail()
{
    return $this->renderView('Product/detail.twig');
}
```

`@Template` や `#[Template]` が関数定義の直前にある場合も、アクションメソッドに紐づくテンプレートとして扱われます。そのため、関数名の行で右クリックしても、メソッド内部で右クリックしても同じように移動できます。

### Twig からコントローラーを開く

Twig テンプレートを開き、右クリックして `EC-CUBE: Open Related Controller Action` を選択します。

拡張機能は、現在の Twig パスを参照している `render()` などの呼び出しをコントローラーファイルから検索し、該当するアクションメソッドへ移動します。

複数の候補が見つかった場合は Quick Pick が表示され、移動先を選択できます。

## テンプレート検索順

カスタムテンプレートディレクトリで即時に一致するファイルが見つからない場合、次のディレクトリを検索し、候補を Quick Pick にまとめて表示します。

| 優先度 | パス |
|---:|---|
| 1 | `app/template/<custom>/` |
| 2 | `app/template/default/` |
| 3 | `src/Eccube/Resource/template/default/` |

`app/template/<custom>/` は、設定値または自動検出されたカスタムテンプレートディレクトリです。

## カスタムテンプレートディレクトリ

`app/template/` 配下に `default` 以外のディレクトリがある場合、拡張機能はカスタムテンプレートディレクトリとして扱います。

明示的にディレクトリを選びたい場合は、コマンドパレットから `EC-CUBE: Select Template Directory` を実行してください。選択した値は `.vscode/settings.json` に保存されます。

```json
{
  "ecCubeNavigator.templateDirectoryName": "custom_theme"
}
```

設定値が空のままでも自動検出されます。複数のカスタムディレクトリがあり、どれを使うか判断できない場合は Quick Pick が表示されます。

## Twig ファイルの新規作成

`@Template`、`#[Template]`、`render()`、`renderView()` で参照されている Twig ファイルが、どの検索ディレクトリにも存在しない場合、Quick Pick の下部に `Create new file` が表示されます。

選択すると、次の場所に Twig ファイルを作成して開きます。

```text
app/template/default/<dir>/<file>.twig
```

親ディレクトリが存在しない場合も自動で作成されます。すでにいずれかの検索ディレクトリにファイルが存在する場合、新規作成の候補は表示されません。

## コンテキストメニュー

| ファイル | コマンド | 動作 |
|---|---|---|
| `.php` | `EC-CUBE: Open Related Twig Template` | 現在のアクションが描画する Twig テンプレートを開く |
| `.twig` | `EC-CUBE: Open Related Controller Action` | 現在の Twig を描画しているコントローラーアクションへ移動する |

どちらのコマンドも、エディター上の右クリックメニューとエクスプローラー上の右クリックメニューから利用できます。

## 設定

VS Code の Settings で `ecCubeNavigator` を検索すると、次の設定を変更できます。

| 設定 | 既定値 | 説明 |
|---|---|---|
| `ecCubeNavigator.templateDirectoryName` | `""` | `app/template/` 配下で優先して検索するカスタムテンプレートディレクトリ名。空の場合は自動検出されます。 |

## 動作要件

- Visual Studio Code `^1.90.0`
- EC-CUBE 4.x の標準的なプロジェクト構成

## 注意事項

- テンプレートの照合は、EC-CUBE のテンプレートディレクトリ内で正規化した Twig パスをもとに行います。
- コントローラーソースに Twig パス文字列が含まれない独自のレンダリング処理では、自動解決できない場合があります。
- 自動解決できない場合や候補が複数ある場合は、Quick Pick から移動先を選択してください。

## Marketplace

- [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=colscenery.ec-cube-controller-twig-navigator)

## Repository

- [GitHub](https://github.com/TakashiHishiki/ec-cube-controller-twig-navigator)

## License

MIT
