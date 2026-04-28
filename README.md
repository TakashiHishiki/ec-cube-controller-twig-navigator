# EC-CUBE Controller Twig Navigator

[![Version](https://img.shields.io/visual-studio-marketplace/v/colscenery.ec-cube-controller-twig-navigator?label=Marketplace)](https://marketplace.visualstudio.com/items?itemName=colscenery.ec-cube-controller-twig-navigator)
[![Installs](https://img.shields.io/visual-studio-marketplace/i/colscenery.ec-cube-controller-twig-navigator)](https://marketplace.visualstudio.com/items?itemName=colscenery.ec-cube-controller-twig-navigator)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE.txt)

EC-CUBE の PHP コントローラーアクションと Twig テンプレートを、エディターの右クリックメニューから素早く行き来できる Visual Studio Code 拡張機能です。

---

## Features

- PHP コントローラーファイルを開いた状態で右クリック → 対応する Twig テンプレートへジャンプ
- Twig テンプレートを開いた状態で右クリック → 対応するコントローラーアクションへジャンプ
- Twig → コントローラーの検索順: `/app/Customize/Controller/` を優先し、なければ `/src/Eccube/Controller/` へフォールバック
- コントローラー → Twig の検索順: `/app/template/<設定したディレクトリ>/` を優先し、なければ `/app/template/default/` へフォールバック
- `render()` / `renderView()` / `@Template(...)` / `#[Template(...)]` の各パターンに対応
- 複数の候補が見つかった場合は Quick Pick リストで選択できる
- `.vscode/settings.json` が存在しない場合は自動作成し、VS Code の通知でお知らせ

---

## Installation

VS Code の Quick Open（`Ctrl+P` / `Cmd+P`）を開き、以下を貼り付けて Enter を押します。

```
ext install colscenery.ec-cube-controller-twig-navigator
```

または [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=colscenery.ec-cube-controller-twig-navigator) から直接インストールできます。

---

## Workspace Setting

`.vscode/settings.json` が存在しない場合、拡張機能が自動的にサンプル付きで作成し、VS Code の通知でお知らせします。

使用するテンプレートディレクトリ名をワークスペースの `.vscode/settings.json` に設定してください。

```json
{
  "ecCubeNavigator.templateDirectoryName": "custom_theme"
}
```

### テンプレートの検索順

| 優先度 | パス |
|:---:|---|
| 1 | `/app/template/<ecCubeNavigator.templateDirectoryName>/` （設定ディレクトリが存在する場合） |
| 2 | `/app/template/default/` （存在する場合） |
| 3 | `/src/Eccube/Resource/template/default/` （上記どちらも存在しない場合のみ） |

---

## How It Works

### コントローラー → Twig

PHP コントローラーファイルを開き、アクションメソッドの中にカーソルを置いて右クリック →  
**`EC-CUBE: Open Related Twig Template`** を選択します。

対応するコントローラーの記述パターン:

```php
$this->render('Product/detail.twig', [...])
$this->renderView('Product/detail.twig', [...])

/**
 * @Template("Product/detail.twig")
 */

#[Template('Product/detail.twig')]
```

コントローラーファイル内の `@Template("Section/action.twig")` の文字列を直接右クリックしてジャンプすることもできます。

---

### Twig → コントローラー

Twig テンプレートを開いて右クリック →  
**`EC-CUBE: Open Related Controller Action`** を選択します。

コントローラーの検索順:

1. `/app/Customize/Controller/` が存在する場合はそちらを優先
2. カスタマイズコントローラーに一致がなければ `/src/Eccube/Controller/` を検索

複数の候補が見つかった場合や自動マッチができなかった場合は、Quick Pick リストが開いて手動で選択できます。Quick Pick の結果は関連度順にソートされます。

---

## Notes

- テンプレートのマッチングは、EC-CUBE の各ディレクトリ内の Twig パスのサフィックスを正規化して比較しています
- コントローラーのソース内に Twig パスの文字列が含まれないカスタムレンダリングフローは対応していません

---

## License

[MIT](./LICENSE.txt)
