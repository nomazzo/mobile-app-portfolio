# iOS Marketing Analysis Template Pack

このテンプレートパックは、iOSアプリのソースコードをCodexで解析し、App Storeメタデータ作成に使えるマーケティング向けドキュメントを作るためのものです。

## 含まれるファイル

- `ios-marketing-feature-analysis-template.md`
  - Codexが埋めるためのテンプレート本体。
- `codex-prompt-ios-marketing-analysis.md`
  - VS CodeでiOSアプリを開いた状態でCodexに渡すプロンプト。
- `chatgpt-next-step-prompt-appstore-metadata.md`
  - Codexが生成した分析書をもとに、ChatGPTでApp Store文言を作るためのプロンプト。

## Android移植用テンプレートとの違い

既存の `01-ios-current-app-analysis-template.md` は、Android化のために画面構成・技術構成・依存関係・移植判断を整理するテンプレートです。

このテンプレートは、App Storeメタデータ作成のために、以下を重視します。

- ユーザー向け機能
- アプリの価値
- 想定ユーザー
- 利用シーン
- 訴求軸
- キーワード素材
- スクリーンショットコピー案
- ローカライズ時の注意
- App Store説明文に使える素材

## 推奨ワークフロー

1. iOSアプリのフォルダをVS Codeで開く。
2. `ios-marketing-feature-analysis-template.md` を `docs/` に置く。
3. `codex-prompt-ios-marketing-analysis.md` のプロンプトをCodexに渡す。
4. Codexに `docs/ios-marketing-feature-analysis.md` を作成させる。
5. 生成された文書をChatGPTに渡す。
6. `chatgpt-next-step-prompt-appstore-metadata.md` を使って、App Store文言を生成する。

## Codex設定の目安

- iOSコード解析: 高
- 古いObjective-C / Storyboard / 複雑なプロジェクト: 高
- 小規模な文書修正: 中

## 注意

- Codexにはソースコードを変更させない。
- ビルド確認は不要。最終ビルド確認はXcodeで人間が行う。
- 使われていない古いコードを現行機能として断定しない。
- App Store向けの誇大表現につながる判断は避ける。
