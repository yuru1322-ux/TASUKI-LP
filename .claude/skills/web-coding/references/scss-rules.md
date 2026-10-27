# SCSS / CSS構成ルール

このファイルは `AI-CODING-RULES.md` の 3章「CSS / SCSS」の内容を、意味・文言・設定値を変更せず
そのまま移行したものです。

## CSS / SCSS

- 装飾にはSCSSを使用する。
- クラス名はBEM記法を使用する。
- 既存のSCSSの書き方・構成・命名規則がある場合は、それを優先する。
- 同じスタイルや処理を何度も記述せず、既存の変数・mixin・共通スタイルを利用する。
- 新しい共通化が必要な場合でも、既存の設計を確認してから行う。
- Figmaにないデザインや装飾をAIの判断で追加しない。

### SCSSファイル構成

* SCSSは、パーツやセクションごとにファイルを分割して管理する。
* 各パーツ・セクションのSCSSは、役割が分かりやすい単位で分割する。
* 各SCSSファイルを`style.scss`で読み込み、最終的に`style.css`としてコンパイルする。
* 共通で使用する変数やmixinなどは、専用のSCSSファイルにまとめて管理する。
* 既存のSCSSファイル構成がある場合は、その構成を優先する。
* 1つのSCSSファイルにすべてのスタイルをまとめない。

例：

```text
scss/
├── style.scss
├── _variables.scss
├── _mixins.scss
├── sections/
│   ├── _01-fv.scss
│   ├── _02-services.scss
│   ├── _03-cases.scss
│   └── ...
└── components/
    ├── _button.scss
    └── _header.scss
```

`style.scss`では各ファイルを読み込む。

```scss
@use "variables";
@use "mixins";

@use "sections/01-fv";
@use "sections/02-services";
@use "sections/03-cases";

@use "components/button";
@use "components/header";
```

各セクション・パーツのスタイルは、それぞれ対応するSCSSファイルに記述する。

### Sassのコンパイル（Live Sass Compiler）

SCSSのコンパイルには、VS Code拡張「Live Sass Compiler」（glenn2223.live-sass）を使用する。

* SCSSを保存すると、`css/style.css` が自動で生成される。
* 設定は `.vscode/settings.json` で管理し、AIが勝手に変更しない。

```json
{
  "liveSassCompile.settings.formats": [
    {
      "format": "compressed",
      "extensionName": ".css",
      "savePath": "/css"
    }
  ],
  "liveSassCompile.settings.generateMap": false
}
```

* 出力は compressed・ソースマップなし。出力先は `css/` 直下（`scss/` の中には出力しない）。
* コンパイル対象は、アンダースコアなしのエントリファイル（`scss/style.scss`）のみ。`_` から始まるファイルはパーシャルとして扱われ、単体では出力されない。
* `css/style.css` は生成物のため、手動で編集しない。
* npm / `package.json` / `package-lock.json` / `node_modules` は使わない。ビルド用コマンド（`sass:build` 等）もない。
* AIはコンパイルを実行できないため、SCSSを変更したあとは、ユーザーにVS Codeで保存（コンパイル）して確認してもらう。
