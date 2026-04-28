# EC-CUBE Controller Twig Navigator

EC-CUBE Controller Twig Navigator は、PHP コントローラーのアクションと Twig テンプレートをエディター上で行き来できる Visual Studio Code 拡張機能です。

---

## 機能

- 右クリックメニューから、現在開いている EC-CUBE コントローラーのアクションに対応する Twig テンプレートを開く。
- 右クリックメニューから、現在開いている Twig テンプレートをレンダリングしているコントローラーのアクションを開く。
- Twig → コントローラーの検索時は `/app/Customize/Controller/` を優先し、見つからない場合は `/src/Eccube/Controller/` にフォールバックする。
- コントローラー → Twig の検索時は `/app/template/<テーマ名>/` を自動検出して優先し、見つからない場合は `/app/template/default/` にフォールバックする。
- `render()`、`renderView()`、`@Template(...)`、`#[Template(...)]` などの一般的なパターンに対応。
- 複数の候補が見つかった場合や自動解決できない場合は、Quick Pick リストを表示して手動で選択できる。

---

## テンプレートディレクトリの自動検出

`/app/template/` 配下にあるカスタムテーマのディレクトリ名は、設定ファイルへの記述なしに自動で検出されます。

**検出ロジック（優先順位）：**

1. `/app/template/` 配下を自動スキャンし、`default` 以外のディレクトリを収集する。
   - カスタムディレクトリが **1つだけ** の場合 → 自動で使用（操作不要）。
   - **複数** ある場合 → Quick Pick で選択できる。
2. テンプレートの検索順序：
   - `/app/template/<検出されたテーマ名>/`
   - `/app/template/default/`
   - 上記どちらも存在しない場合のみ `/src/Eccube/Resource/template/default/`

コマンドパレット（`Ctrl+Shift+P`）から **EC-CUBE: Select Template Directory** を実行すると、テーマディレクトリを一覧から選択して設定に保存することもできます。

---

## コンテキストメニュー（右クリック）

| 開いているファイル | メニュー項目 | 動作 |
|---|---|---|
| `.php` コントローラー | EC-CUBE: Open Related Twig Template | 対応する Twig テンプレートへジャンプ |
| `.twig` テンプレート | EC-CUBE: Open Related Controller Action | レンダリングしているコントローラーのアクションへジャンプ |

エクスプローラーパネルで `.php` / `.twig` ファイルを右クリックした場合も同様に使用できます。

---

## 使い方

### コントローラーのアクション → Twig

PHP コントローラーファイルを開き、アクション内にカーソルを置いて右クリック → **EC-CUBE: Open Related Twig Template** を選択します。

対応しているコントローラーのパターン：

- `$this->render('Product/detail.twig', ...)`
- `$this->renderView('Product/detail.twig', ...)`
- `@Template("Product/detail.twig")`
- `#[Template('Product/detail.twig')]`

コントローラーファイル内の `@Template("Section/action.twig")` 記述を直接右クリックして、そのテンプレートへジャンプすることもできます。

### Twig → コントローラーのアクション

Twig テンプレートを開いて右クリック → **EC-CUBE: Open Related Controller Action** を選択します。

コントローラーの検索順序：

1. `/app/Customize/Controller/` — ディレクトリが存在し、マッチが見つかった場合はコアディレクトリを検索しない。
2. `/src/Eccube/Controller/`

複数の候補が見つかった場合、または自動解決できない場合は Quick Pick リストを表示します。Quick Pick の結果は関連度順に並び替えられます。たとえば `guide` アクションは名前に `guide` を含む Twig ファイルを優先し、`@Template("Forgot/index.twig")` は `index` より `Forgot` を優先します。

---

## 動作要件

- VS Code `^1.90.0`
- EC-CUBE 4 の標準的なプロジェクト構成（`app/template/` および `src/Eccube/Controller/` が存在すること）

---

## 注意事項

- テンプレートのマッチングは、EC-CUBE の各ディレクトリ内の Twig パスの末尾を正規化して行います。
- コントローラーのソースコードに Twig パス文字列が含まれないカスタムレンダリングフローを使用している場合、自動解決できないことがあります。その場合は Quick Pick リストが表示されます。

---

## 拡張機能マーケットプレイス

- [VSCodeマーケットプレイス](https://marketplace.visualstudio.com/items?itemName=colscenery.ec-cube-controller-twig-navigator)

---

## ライセンス

MIT
