# TASUKI-LP Claude Code Rules

## Project Goal

Figmaデザインを忠実に再現したLPを制作する。
参照元：Figma「TASUKI」（`Claude　LP_PC_1440` / `Claude　LP_SP_375`）。
背景・ペルソナ・確定コピー等は `/Users/yuika/Documents/ClaudeCode/TASUKI-CLAUDE-TEST/`
配下の README.md / project-brief.md / personas.md / kpi-web-strategy.md /
wireframe-spec.md / design-guide.md を正とする（本リポジトリとは別ディレクトリ。
相対パスでは辿れないため絶対パスで参照すること）。

## Tech Stack

- HTML
- SCSS
- JavaScript（Vanilla。フレームワーク不使用）
- Sass（VS Code拡張「Live Sass Compiler」でコンパイルする）

`scss/style.scss` を保存すると、`css/style.css` が自動で生成される
（設定は `.vscode/settings.json`。出力は compressed・ソースマップなし）。
npm / package.json は使わない。ビルド用のコマンドもない。

バンドラー（webpack/vite等）は使わない。JSはモジュール分割が必要になった場合のみ
`<script type="module">` + 素のESMで対応し、ビルドツールを追加しない。

## Design Priority

- Figmaデザインを最優先する。
- AI独自のデザイン変更・レイアウト改善提案は禁止。
- Figmaと引き継ぎ資料（wireframe-spec.md等）の記載が食い違う場合はFigmaを正とする。
- `08_Nursing_Rehabilitation_Collaboration`（看護とリハビリを、分けて考えない）は
  セクション自体が削除された。実装対象のセクション番号は00（Header）〜10
  （FINAL CTA）で、08にあたる欠番はない。
- 「要確認」「要支給」の情報（24時間対応の条件、FAX等）を
  それらしい実データで埋めない。プレースホルダーと分かる形（例：
  `※24時間対応の条件は現在確認中です` 等）で残す。
- 以下は確定事項として、Figmaの記載どおりに実装する（09 Partners・10 Final CTA）。
  - 電話番号：`03-1111-2222`（Figma記載値。表記もFigmaのまま。
    10 PC/SPと09 PCは `[電話番号：03-1111-2222]`、09 SPは番号のみ。
    `tel:`リンクは `0311112222`）
  - 受付時間：`9:00-18:00`
  - 対応エリア：八王子周辺

## Claudeに任せてよいこと／勝手に判断しないこと

| Claudeに任せる | 勝手に判断しない・要確認 |
|---|---|
| SCSSの記述方法 | 色・フォントサイズなどの数値 |
| HTMLの細かい構造 | デザインそのものの変更 |
| レスポンシブの補間方法（実装手段） | 未確定情報の補完 |
| コードのリファクタリング（依頼された場合のみ） | 既存画像の差し替え |
| アクセシビリティ改善（依頼された場合のみ） | `08`セクションの実装 |
| 命名規則に沿ったファイル整理（依頼された場合のみ） | 新しいライブラリ・バンドラーの導入 |
| ブレイクポイント間の見た目の補間の実装 | ブレイクポイントの数値そのものを新規に決めること |

判断に迷う場合（デザイン上の根拠が見つからない、複数の解釈がありうる等）は、
仮の判断で進めず、実装を止めて確認する。

## Design Tokens

design-guide.mdの実測値を`_variables.scss`に固定し、実装中に近似値を
作らないようにする。

```scss
// _variables.scss
$color-primary:        #D84A43; // Primary CTA, 看護カード枠, 見出しライン, CASE番号
$color-coral:           #F97265; // 英字セクションタイトル（opacity 85%運用）
$color-rehab-orange:   #E98A4A; // リハビリ領域の補助アクセント
$color-pale-pink:       #FFF8F8; // 03_Cases等の背景
$color-text-primary:   #232323; // H1/H2, 本文主要文字
$color-text-secondary: #444444; // カード説明文
$color-text-meta:       #6B6B6B; // 補足テキスト

$font-family-base: "Noto Sans JP", sans-serif;

$content-max-width: 1200px;
$content-padding-pc: 120px;
$content-padding-sp: 20px;

$breakpoint-sp: 375px;
$breakpoint-pc: 1440px;
```

新しいメインカラー・フォントを追加しない。値を直接書かず、変数を参照する。
ただし、PC 05_Strengthsのゴースト数字にFigma指定の`Roboto`を使用するため、
`Roboto`のみ追加を許可済み（他のフォントは追加しない）。
`_variables.scss`自体は実装開始時にClaudeがFigmaの実測値を確認しながら作成する
（この時点では作らない）。

## Breakpoint方針

- 375px（SP）と1440px（PC）の2つの確定デザインのみが存在する。中間の固定切り替え幅
  （768px等）はFigma上に根拠がないため、現時点では決め打ちしない。
- 375〜1440pxの間は、デザインの意図（余白比率・カラム構成・文字サイズの relationship）
  を維持しながらfluidに調整する。
- Mobile Firstで記述する。
- デザインの意図から判断できない補間方法（極端に狭い/広い場合の挙動など）は、
  Claudeが独自の仕様を新しく作らず、実装を止めて確認する。

## File Structure

以下は実装開始時の目標構成であり、**現時点ではまだ作成しない**。
Figmaの実測値確認とCLAUDE.mdの確定を先に終わらせ、実装開始の指示があってから
Claudeがこの構成でファイルを作成する。セクション実装はFigmaの番号
（`wireframe-spec.md`と一致）に対応させる。

```
scss/
  style.scss          // 各ファイルをimportするエントリ
  _variables.scss
  _mixins.scss
  _base.scss           // reset, base typography
  sections/
    _00-header.scss
    _01-fv.scss
    _02-services.scss
    _03-cases.scss
    _04-trial-rehabilitation.scss
    _05-strengths.scss
    _06-medical-coverage.scss
    _07-staff.scss
    _08-on-call.scss
    _09-for-referrers.scss
    _10-final-cta.scss
css/
  style.css            // ビルド生成物。手動編集しない
js/
  main.js
images/                // 既存アセット（fv.png, riha-long.png, riha-wide.png,
                        // staff1〜6.png）。追加が必要な場合はFigmaから
                        // エクスポートし、セクション名が分かるファイル名にする
index.html
```

## CSS / SCSS

- BEMを基本とする
- Mobile First
- セクションごとにSCSSを分割し、`style.scss`でまとめてimportする
- 共通値（色・余白・フォントサイズ・ブレイクポイント）は`_variables.scss`に集約する
- `!important`は原則使用しない

## Implementation

- セクション単位で実装する（00 Header → 01 → 02 → … → 10 FINAL CTA）
- 着手前に、対象セクションの実測値（余白・フォントサイズ・色・Auto Layout構造）を
  Figmaで確認する
- 実装後にブラウザで確認する。**375pxと1440pxの両方**で確認する
- デザインとの差異を修正してから次のセクションへ進む
- まとめて複数セクションを実装しない

## Assets

- 既存画像（`images/`配下）は決め打ちで参照する。ファイル名を変更しない
- 新規に画像が必要な場合はFigmaからエクスポートし、対象セクションが分かる
  ファイル名にする
- 画像はできるだけ軽量な形式・サイズで書き出す（現状のPNGは1点あたり1.7〜2MB前後
  あり、そのままでは重いため、実装時に圧縮 or WebP変換を検討する）

## Accessibility

医療系サイトのため最低限のアクセシビリティを担保する。

- 全ての画像に適切な`alt`属性を付ける（装飾目的の画像は`alt=""`）
- 見出し階層（h1→h2→h3）を正しくネストする
- コントラスト比はWCAG AA基準を意識する。特に`$color-coral`（#F97265）を
  白背景に細い文字で使う場合はコントラスト不足になりやすいので注意する
- クリック可能要素（CTA等）はキーボード操作でも到達・実行できるようにする

## Git

- リポジトリ：`https://github.com/yuru1322-ux/TASUKI-LP`
- コミットは、ユーザーの明示的な指示があったときのみ行う。指示を受けた場合は、
  セクション単位など意味のある粒度で行う
- **Git pushを勝手に行わない**（明示的な指示があるまでpushしない）
- **force push・mainへの直接pushもしない**
- ブランチを切って作業する場合は、命名規則をKOMAさんと事前に決める

## Do Not

- 不要なライブラリ・バンドラーを追加しない
- ファイルを勝手に削除しない
- デザインを勝手に変更しない
- Git pushを勝手に行わない
- 「要確認」「要支給」の情報（確定済みの電話番号・受付時間・対応エリアを除く）を
  それらしい実データで埋めない

## AI Coding Rules

@AI-CODING-RULES.md
