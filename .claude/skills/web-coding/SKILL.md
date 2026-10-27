---
name: web-coding
description: >
  TASUKI-LPのHTML / SCSS(CSS) / Vanilla JavaScriptの実装・修正・レビューを行うときに使用する。
  命名規則（BEM・camelCase）、SCSSファイル構成とLive Sass Compilerの設定、変更範囲の扱い方、
  コードレビュー前の確認事項、セキュリティ、Git操作の制約、崩れないUI実装（画像・レイアウト・
  フォーム・ナビゲーション・リンク・JS）のルールを持つ。WordPress/CMS化の作業を行うときは
  references/wordpress-rules.md も合わせて使用する。Figmaデザインの検討・提案そのもの
  （コードを書く前段階のデザイン作業）には使用しない。
---

# Web Coding Skill

TASUKI-LPでHTML / SCSS / Vanilla JavaScriptのコーディング作業を行う際に使用するSkill。
このSKILL.mdはルールの本体ではなく、作業内容に応じて読むべき`references/`内のファイルを
判断するための入口（司令塔）として使用する。詳細なルールはここに書かず、必ず該当する
referenceファイルを読むこと。

## 使用するタイミング

- HTML / SCSS(CSS) / Vanilla JavaScriptの実装・修正・リファクタリングを行うとき
- 既存コードのレビュー・影響範囲確認を行うとき
- レスポンシブ対応・UIの崩れやすさを検討するとき
- WordPress化・CMS化の作業を行うとき

次のような作業では使用しない（対象外）。

- Figma上でのデザイン検討・デザイン提案そのもの（コーディングに入る前の工程）
- コーディングと無関係な文書作成・調査作業

## 読むべきreferenceの判断

作業内容に応じて、以下の表に沿って必要なreferenceだけを読むこと。複数該当する場合は該当するものをすべて読む。

| 作業内容 | 読むreference |
|---|---|
| どんなコーディング作業でも共通して守るべき基本ルール（基本方針・命名規則・既存コードの扱い・変更範囲・変更前の確認・セキュリティ・Git操作・判断に迷う場合の対応・完了報告・実装手順） | `references/coding-rules.md` |
| SCSS/CSSの記述・ファイル分割・Live Sass Compilerまわりの作業 | `references/scss-rules.md` |
| レスポンシブ・可変コンテンツ・画像/フォーム/ナビゲーション/固定要素/リンク/JS等、UIが崩れないようにする実装 | `references/robust-ui-rules.md` |
| WordPress化・CMS化の指示があったときのみ | `references/wordpress-rules.md`（指示がない限り読む必要はなく、内容を実装に反映してもいけない） |

### 基本の読み方

1. どんなコーディング作業でも、まず `references/coding-rules.md` を読む。
2. SCSS/CSSを書く・直す作業が含まれる場合は `references/scss-rules.md` も読む。
3. UIの実装（特にレスポンシブ、画像、フォーム、ナビゲーション、固定要素、リンク、JS）が含まれる場合は `references/robust-ui-rules.md` も読む。
4. ユーザーからWordPress化・CMS化の明示的な指示があった場合のみ、該当する範囲で `references/wordpress-rules.md` を読む。指示がない限りこのファイルは読まず、内容も適用しない。

## このSkillの原典について

`references/`配下の各ファイルは、プロジェクトの `AI-CODING-RULES.md` を原典とし、内容を要約・一般化せずに分割・移行したもの。ルールの内容そのものを変更する場合は、まず`AI-CODING-RULES.md`側の扱いを確認すること。
