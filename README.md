# EC-CUBE Controller Twig Navigator

EC-CUBE 開発者向けの Visual Studio Code 拡張機能です。コントローラーのアクションメソッドと Twig テンプレートを右クリック一発で相互に行き来できます。

[![VS Marketplace](https://img.shields.io/badge/VS%20Marketplace-EC--CUBE%20Controller%20Twig%20Navigator-blue?logo=visual-studio-code)](https://marketplace.visualstudio.com/items?itemName=colscenery.ec-cube-controller-twig-navigator)

🔗 [拡張機能マーケットプレイスで見る](https://marketplace.visualstudio.com/items?itemName=colscenery.ec-cube-controller-twig-navigator)

---

## 機能

### コントローラー → Twig

PHP コントローラーを開いた状態で、アクションメソッドの内部（またはメソッド名の行）を右クリックし、**EC-CUBE: Open Related Twig Template** を選択します。

カーソル位置のアクションに対して、次の 2 種類のテンプレート参照を同時にスキャンします。

1. **`@Template` アノテーション / PHP 8 アトリビュート** — 関数定義の直前に書かれた `@Template("dir/file.twig")` または `#[Template("dir/file.twig")]`
2. **`render()` / `renderView()` 呼び出し** — メソッド本体内の `$this->render('dir/file.twig')`

カーソルをメソッド名の行に置いても、`@Template` の行や本体内に置いた場合とまったく同じように動作します。

#### 解決の挙動

**カスタムテンプレートディレクトリが存在する場合（`app/template/<カスタム>/`）**

参照先ファイルが `app/template/<カスタム>/<dir>/<file>.twig` に見つかった場合は、ピッカーを表示せず即座に開きます。

**カスタムディレクトリが存在しない、またはファイルが見つからない場合**

全検索ディレクトリ（後述のテンプレート検索順を参照）から候補を集めた Quick Pick リストを表示します。リスト下部のセパレーター以降には **Create new file** エントリーが表示され、選択すると `app/template/default/` に空のファイル（必要な親ディレクトリを含む）を自動生成してエディターで開きます。

---

### Twig → コントローラー

Twig テンプレートを開いた状態で右クリックし、**EC-CUBE: Open Related Controller Action** を選択します。

コントローラーファイルを検索して、現在のテンプレートパスを参照している `render()` 呼び出しを持つアクションメソッドへジャンプします。複数一致した場合は Quick Pick リストを表示します。

---

### テンプレートディレクトリの選択

コマンドパレット（`Ctrl+Shift+P` / `Cmd+Shift+P`）から **EC-CUBE: Select Template Directory** を実行すると、`app/template/` 配下のどのサブディレクトリを優先検索対象にするかを選択できます。選択結果は `.vscode/settings.json` に保存されます。

---

## テンプレート検索順

カスタムディレクトリで即時一致が得られなかった場合、次の順番で全ディレクトリを検索し、結果を 1 つの Quick Pick リストにまとめます。

| 優先度 | パス |
|:---:|---|
| 1 | `app/template/<カスタム>/`（設定済みまたは自動検出） |
| 2 | `app/template/default/` |
| 3 | `src/Eccube/Resource/template/default/` |

---

## カスタムテンプレートディレクトリのセッションキャッシュ（v0.0.7）

`.vscode/settings.json` に `ecCubeNavigator.templateDirectoryName` が未設定で、かつ `app/template/` 配下に複数のカスタムディレクトリが存在する場合、v0.0.6 まではナビゲーションコマンドを実行するたびにディレクトリ選択の Quick Pick が表示されていました。そのため、目的のファイルに到達するまでに 2 回のクリックが必要でした。

**v0.0.7 からは、選択結果を VS Code のセッション中にキャッシュします。** プロンプトはセッション内で 1 回だけ表示され、それ以降のコマンドではキャッシュされたディレクトリ名を再利用するため、1 回のクリックで直接移動できます。

**EC-CUBE: Select Template Directory** を実行して永続設定を保存した際は、キャッシュが自動的にクリアされます。

---

## 新規ファイルの作成

`@Template` または `render()` で参照されているテンプレートパスがどの検索ディレクトリにも存在しない場合、Quick Pick リストの末尾に **Create new file** エントリーが表示されます。

- 作成先は常に `app/template/default/<dir>/<file>.twig`
- 不足している親ディレクトリも自動的に作成されます
- 作成後、ファイルがすぐにエディターで開きます

いずれかのディレクトリにファイルが既に存在する場合、このエントリーは表示されません。

---

## コンテキストメニューコマンド

| ファイル種別 | コマンド | 動作 |
|---|---|---|
| `.php` | EC-CUBE: Open Related Twig Template | 現在のアクションがレンダリングする Twig テンプレートを開く |
| `.twig` | EC-CUBE: Open Related Controller Action | このテンプレートをレンダリングするコントローラーアクションへジャンプ |

いずれのコマンドもエクスプローラーのコンテキストメニューからも利用できます。

---

## 設定

設定（`Ctrl+,`）を開いて `ecCubeNavigator` で検索します。

| 設定キー | デフォルト | 説明 |
|---|---|---|
| `ecCubeNavigator.templateDirectoryName` | `""` | `app/template/` 配下のカスタムテンプレートディレクトリ名。空にすると自動検出します。複数のディレクトリが見つかり未設定の場合は、セッション中に 1 回だけ Quick Pick が表示され、永続設定を保存するまでキャッシュされます。 |

`.vscode/settings.json` への記述例:

```json
{
  "ecCubeNavigator.templateDirectoryName": "your_theme"
}
```

---

## 動作要件

- VS Code `^1.90.0`
- EC-CUBE 4.x の標準的なプロジェクト構成

---

## インストール

**VS Code Quick Open**（`Ctrl+P`）を開き、以下を貼り付けて Enter を押します。

```
ext install colscenery.ec-cube-controller-twig-navigator
```

または [拡張機能マーケットプレイス](https://marketplace.visualstudio.com/items?itemName=colscenery.ec-cube-controller-twig-navigator) から直接インストールできます。

---

## 変更履歴

### 0.0.7

- **修正: カスタムテンプレートディレクトリの選択プロンプトがナビゲーションのたびに表示される問題を修正。** `ecCubeNavigator.templateDirectoryName` が未設定で `app/template/` 配下に複数のカスタムディレクトリが存在する場合、ディレクトリ選択の Quick Pick がコマンド実行のたびに表示され、目的のファイルへ到達するまでに 2 回のクリックが必要でした。選択結果を VS Code セッション中にキャッシュするよう変更し、プロンプトはセッション内で 1 回だけ表示されるようになりました。

### 0.0.6

- カーソルをアクションメソッドの任意の位置（関数シグネチャの行を含む）に置いても、関数定義の直前に書かれた `@Template` アノテーションが正しく認識されるようになりました。メソッド名を右クリックした場合も、`@Template` の行や本体内を右クリックした場合とまったく同じ動作になります。
- `app/template/<カスタム>/` が存在し、参照先テンプレートがそこに見つかった場合、ピッカーを表示せず即座に開くようになりました。
- カスタムディレクトリで一致が得られなかった場合、全検索ディレクトリ（`app/template/default/` および `src/Eccube/Resource/template/default/`）の候補と **Create new file** エントリーをまとめた Quick Pick リストを表示するようになりました。

### 0.0.5

- 初回公開リリース。

---

## ライセンス

[MIT](https://github.com/TakashiHishiki/ec-cube-controller-twig-navigator/blob/HEAD/LICENSE.txt)
