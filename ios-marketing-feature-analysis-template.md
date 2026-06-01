# {{APP_NAME}} - iOS Marketing Feature Analysis

作成日: {{YYYY-MM-DD}}
解析対象: `{{IOS_PROJECT_ROOT}}`
作成者/作成AI: Codex
目的: App Store / Google Play のメタデータ作成、プロモーションテキスト、説明文、キーワード、スクリーンショットコピー作成に使うためのマーケティング向け機能分析。

> この文書は、Android移植仕様書ではない。実装詳細を深掘りしすぎず、ユーザーが体験する価値・機能・訴求ポイント・ASO素材に変換しやすい情報を整理する。

---

## 0. 解析メタ情報

| 項目 | 内容 |
| --- | --- |
| 現在のアプリ名 | {{CURRENT_APP_NAME_OR_UNKNOWN}} |
| Bundle ID | {{BUNDLE_ID_OR_UNKNOWN}} |
| 想定カテゴリ | {{CATEGORY_OR_UNKNOWN}} |
| 対応プラットフォーム | iOS |
| 主な実装言語 | {{SWIFT_OBJECTIVEC_ETC}} |
| 主要画面定義 | {{STORYBOARD_XIB_SWIFTUI_OR_UNKNOWN}} |
| 解析対象の主なパス | {{MAIN_SOURCE_PATHS}} |
| 解析できなかった範囲 | {{UNKNOWN_OR_NOT_CHECKED}} |

---

## 1. アプリの一言要約

### 1.1 ユーザー向け一言説明

{{APP_ONE_LINE_DESCRIPTION_FOR_USERS}}

例: 料理写真をすばやく撮影・編集・保存できる、シンプルなフードカメラアプリ。

### 1.2 アプリの本質

このアプリの本質は、以下である。

- {{CORE_VALUE_1}}
- {{CORE_VALUE_2}}
- {{CORE_VALUE_3}}

### 1.3 何のアプリではないか

誤解を避けるため、以下のようなアプリとしては訴求しない。

- {{NOT_THIS_KIND_OF_APP_1}}
- {{NOT_THIS_KIND_OF_APP_2}}

---

## 2. 想定ユーザーと利用シーン

### 2.1 想定ユーザー

| ユーザー像 | ニーズ | このアプリが提供する価値 |
| --- | --- | --- |
| {{USER_PERSONA_1}} | {{NEED_1}} | {{VALUE_1}} |
| {{USER_PERSONA_2}} | {{NEED_2}} | {{VALUE_2}} |
| {{USER_PERSONA_3}} | {{NEED_3}} | {{VALUE_3}} |

### 2.2 主な利用シーン

- {{USE_CASE_1}}
- {{USE_CASE_2}}
- {{USE_CASE_3}}
- {{USE_CASE_4}}

### 2.3 使用頻度の想定

- 想定頻度: {{DAILY_WEEKLY_OCCASIONAL}}
- 理由: {{USAGE_FREQUENCY_REASON}}

---

## 3. ユーザー向け主要機能

> 実装クラス名や内部処理ではなく、ユーザーが理解できる機能名で記載する。各機能には、根拠となるファイル・画面・関数を簡潔に添える。

| 優先度 | 機能名 | ユーザーにとっての価値 | 根拠ファイル/画面/関数 | 訴求に使えるか |
| --- | --- | --- | --- | --- |
| 高 | {{FEATURE_NAME_1}} | {{USER_VALUE_1}} | `{{EVIDENCE_PATH_1}}` | Yes/No |
| 高 | {{FEATURE_NAME_2}} | {{USER_VALUE_2}} | `{{EVIDENCE_PATH_2}}` | Yes/No |
| 中 | {{FEATURE_NAME_3}} | {{USER_VALUE_3}} | `{{EVIDENCE_PATH_3}}` | Yes/No |
| 低 | {{FEATURE_NAME_4}} | {{USER_VALUE_4}} | `{{EVIDENCE_PATH_4}}` | Yes/No |

### 3.1 最も強く訴求すべき機能

1. {{TOP_FEATURE_1}}
   - 理由: {{REASON_1}}
2. {{TOP_FEATURE_2}}
   - 理由: {{REASON_2}}
3. {{TOP_FEATURE_3}}
   - 理由: {{REASON_3}}

### 3.2 訴求しすぎないほうがよい機能

| 機能 | 理由 |
| --- | --- |
| {{WEAK_OR_RISKY_FEATURE_1}} | {{REASON}} |
| {{WEAK_OR_RISKY_FEATURE_2}} | {{REASON}} |

---

## 4. 画面・ユーザーフロー分析

### 4.1 主要画面

| 画面名 | ユーザーができること | スクショ候補 | 根拠 |
| --- | --- | --- | --- |
| {{SCREEN_1}} | {{ACTION_1}} | Yes/No | `{{EVIDENCE}}` |
| {{SCREEN_2}} | {{ACTION_2}} | Yes/No | `{{EVIDENCE}}` |
| {{SCREEN_3}} | {{ACTION_3}} | Yes/No | `{{EVIDENCE}}` |

### 4.2 基本フロー

```text
{{STEP_1}}
↓
{{STEP_2}}
↓
{{STEP_3}}
↓
{{STEP_4}}
```

### 4.3 スクリーンショットで見せるべき流れ

1. {{SCREENSHOT_STORY_1}}
2. {{SCREENSHOT_STORY_2}}
3. {{SCREENSHOT_STORY_3}}
4. {{SCREENSHOT_STORY_4}}
5. {{SCREENSHOT_STORY_5}}

---

## 5. 差別化ポイント・強み

### 5.1 競合と比べたときの訴求軸候補

> Codexは競合調査をしない。ここでは、コード・UI・既存機能から見える訴求軸だけを書く。競合比較として断定しない。

- {{DIFFERENTIATION_ANGLE_1}}
- {{DIFFERENTIATION_ANGLE_2}}
- {{DIFFERENTIATION_ANGLE_3}}

### 5.2 アプリの雰囲気・ブランドトーン

| 項目 | 内容 |
| --- | --- |
| 印象 | {{SIMPLE_PROFESSIONAL_FUN_RETRO_ETC}} |
| 色・見た目 | {{VISUAL_TONE}} |
| コピーの方向性 | {{COPY_TONE}} |
| 避けたい表現 | {{WORDS_TO_AVOID}} |

---

## 6. App Store メタデータ作成のための素材

### 6.1 App名候補の方向性

> ここでは最終案ではなく、ChatGPTに名前案を作らせるための方向性を出す。

- 現在名を活かす方向: {{NAME_DIRECTION_KEEP_CURRENT}}
- 機能を明確にする方向: {{NAME_DIRECTION_FUNCTIONAL}}
- 検索語を入れる方向: {{NAME_DIRECTION_ASO}}
- ブランド寄りの方向: {{NAME_DIRECTION_BRAND}}

### 6.2 サブタイトル向け訴求軸

- {{SUBTITLE_ANGLE_1}}
- {{SUBTITLE_ANGLE_2}}
- {{SUBTITLE_ANGLE_3}}

### 6.3 プロモーションテキスト向け訴求軸

- {{PROMO_ANGLE_1}}
- {{PROMO_ANGLE_2}}
- {{PROMO_ANGLE_3}}

### 6.4 Descriptionに入れるべき内容

#### 冒頭で伝えること

{{DESCRIPTION_OPENING_MESSAGE}}

#### 本文で説明する機能

- {{DESCRIPTION_FEATURE_1}}
- {{DESCRIPTION_FEATURE_2}}
- {{DESCRIPTION_FEATURE_3}}
- {{DESCRIPTION_FEATURE_4}}

#### 最後に促す行動

{{DESCRIPTION_CLOSING_MESSAGE}}

### 6.5 Keywords作成用の素材語

> ここではキーワード候補の素材だけを出す。最終的な100文字以内のキーワード列はChatGPT側で重複削除・優先順位付けして作る。

| 種類 | 候補語 |
| --- | --- |
| 機能系 | {{FUNCTION_KEYWORDS}} |
| カテゴリ系 | {{CATEGORY_KEYWORDS}} |
| 用途系 | {{USE_CASE_KEYWORDS}} |
| 対象ユーザー系 | {{USER_KEYWORDS}} |
| 雰囲気/スタイル系 | {{STYLE_KEYWORDS}} |
| 避けるべき語 | {{AVOID_KEYWORDS}} |

---

## 7. スクリーンショットコピー作成のための素材

### 7.1 スクショ1枚目の最有力訴求

{{SCREENSHOT_1_MAIN_PROMISE}}

### 7.2 5枚構成案

| 枚数 | 伝える内容 | 画面/素材 | キャッチコピー方向性 |
| --- | --- | --- | --- |
| 1 | {{SHOT_1_CONTENT}} | {{SHOT_1_SCREEN}} | {{SHOT_1_COPY_ANGLE}} |
| 2 | {{SHOT_2_CONTENT}} | {{SHOT_2_SCREEN}} | {{SHOT_2_COPY_ANGLE}} |
| 3 | {{SHOT_3_CONTENT}} | {{SHOT_3_SCREEN}} | {{SHOT_3_COPY_ANGLE}} |
| 4 | {{SHOT_4_CONTENT}} | {{SHOT_4_SCREEN}} | {{SHOT_4_COPY_ANGLE}} |
| 5 | {{SHOT_5_CONTENT}} | {{SHOT_5_SCREEN}} | {{SHOT_5_COPY_ANGLE}} |

### 7.3 Before/Afterや作例が必要か

- 必要度: {{HIGH_MEDIUM_LOW}}
- 理由: {{REASON}}
- 推奨素材: {{RECOMMENDED_ASSETS}}

---

## 8. 課金・広告・プレミアム機能の扱い

### 8.1 収益化要素

| 種類 | 有無 | ユーザー向け説明に使うか | 備考 |
| --- | --- | --- | --- |
| 広告 | {{YES_NO_UNKNOWN}} | 通常は使わない | {{NOTES}} |
| 広告非表示IAP | {{YES_NO_UNKNOWN}} | 必要なら短く | {{NOTES}} |
| サブスク | {{YES_NO_UNKNOWN}} | {{YES_NO}} | {{NOTES}} |
| リワード広告 | {{YES_NO_UNKNOWN}} | 機能価値がある場合のみ | {{NOTES}} |

### 8.2 メタデータで避けるべきこと

- {{MONETIZATION_COPY_WARNING_1}}
- {{MONETIZATION_COPY_WARNING_2}}

---

## 9. ローカライズ時の注意

### 9.1 翻訳しないほうがよい語

- {{DO_NOT_TRANSLATE_1}}
- {{DO_NOT_TRANSLATE_2}}

### 9.2 国・言語によって表現を変えるべき内容

- {{LOCALIZATION_NOTE_1}}
- {{LOCALIZATION_NOTE_2}}

### 9.3 英語マスターコピーの方向性

- Tone: {{ENGLISH_TONE}}
- Avoid: {{ENGLISH_AVOID}}

---

## 10. 証拠・根拠メモ

> 後で確認できるよう、主要判断の根拠を残す。行番号が取得できる場合は行番号も書く。

| 判断 | 根拠 |
| --- | --- |
| {{CLAIM_1}} | `{{FILE_PATH}}` / {{SYMBOL_OR_SCREEN}} |
| {{CLAIM_2}} | `{{FILE_PATH}}` / {{SYMBOL_OR_SCREEN}} |
| {{CLAIM_3}} | `{{FILE_PATH}}` / {{SYMBOL_OR_SCREEN}} |

---

## 11. 未確認・手動確認が必要なこと

- {{MANUAL_CHECK_1}}
- {{MANUAL_CHECK_2}}
- {{MANUAL_CHECK_3}}

---

## 12. ChatGPTへ渡すときの要約

以下をChatGPTに渡すと、App Store用メタデータを作成しやすい。

```text
アプリ名: {{APP_NAME}}
カテゴリ: {{CATEGORY}}
一言説明: {{ONE_LINE}}
主なユーザー: {{USERS}}
主な機能: {{FEATURES}}
最重要訴求: {{TOP_PROMISE}}
スクショで見せるべき内容: {{SCREENSHOT_STORY}}
キーワード素材: {{KEYWORD_MATERIALS}}
避けるべき訴求: {{AVOID}}
```
