# Magic Voice - iOS Marketing Feature Analysis

作成日: 2026-05-08
解析対象: `app/soundrecordeffects`
作成者/作成AI: Codex
目的: App Store メタデータ、プロモーションテキスト、説明文、キーワード、スクリーンショットコピー作成に使うためのマーケティング向け機能分析。

> この文書は、Android移植仕様書ではない。実装詳細を深掘りしすぎず、ユーザーが体験する価値・機能・訴求ポイント・ASO素材に変換しやすい情報を整理する。

---

## 0. 解析メタ情報

| 項目 | 内容 |
| --- | --- |
| 現在のアプリ名 | Magic Voice |
| Bundle ID | `black.appextreme.voicechanger` |
| 想定カテゴリ | Utilities。音声録音・ボイスチェンジャー系としても訴求可能 |
| 対応プラットフォーム | iOS / iPad対応設定あり |
| 主な実装言語 | Swift 5、UIKit、Storyboard |
| 主要画面定義 | `Base.lproj/Main1.storyboard`、`Launch Screen.storyboard`、一部モーダルはコード生成 |
| 解析対象の主なパス | `app/soundrecordeffects`、`app/soundrecordeffects.xcodeproj/project.pbxproj`、`localization`、旧スクショのみ `MagicVoice_Old/1.MagicVoice/images/*.png` |
| 解析できなかった範囲 | 実機/Xcodeビルド、App Store ConnectのIAP設定、現在のストア審査状態、実広告配信状態、録音音質の実聴確認 |

根拠:
- アプリ名: `app/soundrecordeffects/Info.plist` の `CFBundleName = Magic Voice`、`project.pbxproj` の `INFOPLIST_KEY_CFBundleDisplayName = "Magic Voice"`
- Bundle ID: `app/soundrecordeffects.xcodeproj/project.pbxproj` の `PRODUCT_BUNDLE_IDENTIFIER = black.appextreme.voicechanger`
- カテゴリ: `project.pbxproj` の `INFOPLIST_KEY_LSApplicationCategoryType = "public.app-category.utilities"`
- マイク権限: `Info.plist` の `NSMicrophoneUsageDescription = "Microphone permission needed to record."`

---

## 1. アプリの一言要約

### 1.1 ユーザー向け一言説明

録音した声を、スピードやピッチ、キャラクター風エフェクトで手軽に変えて再生・共有できるボイスレコーダーアプリ。

### 1.2 アプリの本質

- その場で声を録音し、すぐに変声して遊べる。
- 通常再生、速い/遅い再生、高い/低い声、カスタム調整をワンタップで試せる。
- 作成した録音を一覧で管理し、必要に応じて共有できる。

### 1.3 何のアプリではないか

- 写真/カメラ/動画編集アプリではない。写真・カメラ権限や画像編集画面は確認できない。
- AI音声生成、音声合成、テキスト読み上げアプリとしては訴求しない。
- プロ向けDAW、マルチトラック編集、波形編集ツールとしては訴求しない。

---

## 2. 想定ユーザーと利用シーン

### 2.1 想定ユーザー

| ユーザー像 | ニーズ | このアプリが提供する価値 |
| --- | --- | --- |
| 友だちや家族と音声で遊びたい人 | 声を変えてすぐ聞かせたい | 録音後すぐに複数エフェクトを試せる |
| SNS/メッセージで短い音声を共有したい人 | 面白い音声素材を作りたい | `UIActivityViewController` で録音ファイルを共有できる |
| シンプルな録音メモを残したい人 | 余計な編集なしで録音を保存したい | 変声しなくても通常の録音一覧として使える |

### 2.2 主な利用シーン

- 自分の声を録音して、速い声・遅い声・高い声・低い声に変えて聞く。
- 家族や友人との会話、パーティー、子ども向けの遊びで短い音声を作る。
- 録音を一覧から選び、あとで再生・変声・共有する。
- 不要になった録音を削除し、名前を変更して整理する。

### 2.3 使用頻度の想定

- 想定頻度: 必要な時に使うカジュアル利用。
- 理由: 録音・変声・共有の即時性が中心で、毎日の常駐ツールというより「遊びたい時」「音声を残したい時」の起動が自然。

---

## 3. ユーザー向け主要機能

| 優先度 | 機能名 | ユーザーにとっての価値 | 根拠ファイル/画面/関数 | 訴求に使えるか |
| --- | --- | --- | --- | --- |
| 高 | ワンタップ録音 | すぐ声を録れる。録音中はタイマーで経過時間が見える | `ViewController.recordAct(_:)`、`ViewController.updateTime()`、`Main1.storyboard` Record画面 | Yes |
| 高 | 録音後すぐ変声再生 | 録った声をその場で違う声として楽しめる | `ViewController.stopAct(_:)` から `goListen`、`listenView.playSound(_:)` | Yes |
| 高 | 複数の音声エフェクト | 通常、速い、遅い、高い声、低い声、さらに低い声、カスタムを選べる | `listenView.effects`、`normalAct`、`fastAct`、`slowAct`、`babyAct`、`scaryAct`、`robotAct`、`customAct` | Yes |
| 高 | スピード/ピッチのカスタム調整 | 既定エフェクトだけでなく、自分好みの変声を作れる | `speedSlider`、`pitchSlider`、`speedValueChanged(_:)`、`pitchValueChanged(_:)` | Yes |
| 中 | 録音一覧管理 | 過去の録音を選んで再生・変声できる | `UITableView`、`recordsCell`、`FileManager.contentsOfDirectory(atPath:)` | Yes |
| 中 | 録音名の変更/削除 | 作成した録音を整理できる | `tableView(_:editActionsForRowAt:)` の `Rename` / `Delete` | Yes。ただし説明文では補助機能扱い |
| 中 | 音声ファイル共有 | 作った音声を他アプリへ送れる | `listenView.shareApp(_:)`、`UIActivityViewController(activityItems: [URL])` | Yes |
| 低 | 広告非表示 | 広告を一時/永久に非表示にできる | `NoAdsViewController`、`AdRemovalManager`、`Localizable.strings` | メイン訴求にはしない |
| 低 | 開発者の他アプリ導線 | 他アプリへのリンクを開く想定 | `ViewController.moreApps(_:)`、`moreAppsLink = ""` | No。現状リンク空のため未確認 |

### 3.1 最も強く訴求すべき機能

1. 録音してすぐ声を変えられる
   - 理由: 旧説明文、現行画面、`recordAct` から `listenView` への流れがすべて一致している。
2. 速さとピッチで遊べる
   - 理由: 速い/遅い再生、ピッチ変更、カスタムスライダーが実装・画面表示ともに確認できる。
3. 保存済み録音を共有できる
   - 理由: 録音は `Library/sounds` に保存され、共有ボタンからファイルURLを送れる。

### 3.2 訴求しすぎないほうがよい機能

| 機能 | 理由 |
| --- | --- |
| 「無限に声を作れる」 | カスタム調整はあるが、App Store文言としては誇大に見えやすい。「自由に調整」程度が安全 |
| 「プロ品質」 | 録音設定は中品質のAAC/CAFで、プロ向け編集機能は確認できない |
| 「AI」 | AI処理、生成音声、話者変換モデルは確認できない |
| 「広告なし」 | 広告非表示はIAP/リワード要素。無料で広告なしと誤解される表現は避ける |

---

## 4. 画面・ユーザーフロー分析

### 4.1 主要画面

| 画面名 | ユーザーができること | スクショ候補 | 根拠 |
| --- | --- | --- | --- |
| Record画面 | 録音開始/停止、録音時間確認、保存済み録音の一覧選択 | Yes | `Main1.storyboard` / `ViewController` |
| Listen画面 | 録音の再生、エフェクト選択、スピード/ピッチ調整、共有 | Yes | `Main1.storyboard` / `listenView` |
| No Adsモーダル | 広告非表示の購入、30分広告非表示リワード、購入復元 | 条件付き | `NoAdsViewController` |
| 広告同意/Privacy Options | 広告同意管理、必要地域でプライバシー選択 | No | `AdMobConsentManager` / `privacyOptionsButton` |

### 4.2 基本フロー

```text
Record画面で録音ボタンをタップ
↓
録音中にタイマー表示
↓
停止すると録音ファイルを保存し、Listen画面へ遷移
↓
エフェクトボタンまたはスライダーで声を変えて再生
↓
共有ボタンから録音ファイルを他アプリへ送る
```

### 4.3 スクリーンショットで見せるべき流れ

1. 大きなマイクボタンと録音タイマーで「すぐ録れる」ことを見せる。
2. 録音一覧で「録った声を保存してあとから選べる」ことを見せる。
3. Listen画面の8つのアイコンで「ワンタップ変声」を見せる。
4. Speed/Pitchスライダーで「自分好みに調整できる」ことを見せる。
5. 共有ボタンで「作った音声を送れる」ことを見せる。

旧スクリーンショットでは、Record画面に大きなマイク/停止ボタン、録音一覧、Listen画面に大きな再生ボタン、8個のエフェクトアイコン、Speed/Pitchスライダー、共有ボタン、広告枠が表示されていた。現行Storyboardも同系統の構成。

---

## 5. 差別化ポイント・強み

### 5.1 競合と比べたときの訴求軸候補

> 競合調査はしていないため、ここではコード・UI・既存機能から見える訴求軸だけを書く。

- 録音から変声再生までが短い。停止後すぐListen画面へ遷移する。
- 既定エフェクトとカスタム調整の両方がある。
- 音声ファイルを録音一覧に保存し、あとから選んで遊べる。
- 暗色背景とシアンの発光系UIで、音声・波形・ガジェット感を打ち出しやすい。

### 5.2 アプリの雰囲気・ブランドトーン

| 項目 | 内容 |
| --- | --- |
| 印象 | カジュアル、遊びやすい、ややレトロなボイスチェンジャー |
| 色・見た目 | ダークグレー背景、シアンのアイコン/ライン、波形背景 |
| コピーの方向性 | 「録って、変えて、聞いて、シェア」の短く楽しい表現 |
| 避けたい表現 | AI、プロ品質、完全無料、広告なし、スタジオ級、声を完全に別人にする等の断定 |

---

## 6. App Store メタデータ作成のための素材

### 6.1 App名候補の方向性

- 現在名を活かす方向: `Magic Voice` を残し、サブタイトルでボイスチェンジャーと録音を補足。
- 機能を明確にする方向: Voice Changer / Voice Recorder / Sound Effects を組み合わせる。
- 検索語を入れる方向: voice changer、recorder、sound effects、pitch、speed、share。
- ブランド寄りの方向: Magic Voice を短く覚えやすい名称として維持。

### 6.2 サブタイトル向け訴求軸

- Record and change your voice
- Fun voice recorder effects
- Pitch, speed and share sounds
- 録音して声を変える、楽しいボイスチェンジャー

### 6.3 プロモーションテキスト向け訴求軸

- 録音した声をすぐに再生し、速さやピッチを変えて楽しめる。
- 通常のボイスレコーダーとしても使え、保存した録音をあとから選べる。
- 作った音声を共有して、友だちや家族と楽しめる。

### 6.4 Descriptionに入れるべき内容

#### 冒頭で伝えること

Magic Voiceは、声を録音して、スピードやピッチを変えながら楽しく再生できるボイスレコーダー/ボイスチェンジャー。

#### 本文で説明する機能

- ワンタップ録音と録音タイマー。
- 保存された録音の一覧表示。
- 通常、速い、遅い、高い声、低い声などの音声エフェクト。
- Speed/Pitchスライダーによるカスタム調整。
- 作成した録音ファイルの共有。
- 録音名の変更、不要な録音の削除。
- 広告表示があること、広告非表示オプションがあることは必要に応じて別枠で控えめに説明。

#### 最後に促す行動

声を録って、変えて、聞いて、気に入った音声をシェアしよう。

### 6.5 Keywords作成用の素材語

| 種類 | 候補語 |
| --- | --- |
| 機能系 | voice changer, voice recorder, recorder, audio recorder, sound recorder, sound effects, pitch, speed, playback, share |
| カテゴリ系 | utility, entertainment, audio, sound, recording |
| 用途系 | funny voice, change voice, record voice, voice effect, prank sound, share audio |
| 対象ユーザー系 | friends, family, kids, casual users |
| 雰囲気/スタイル系 | magic, fun, simple, playful, quick |
| 避けるべき語 | AI, professional studio, celebrity voice, free no ads, text to speech |

---

## 7. スクリーンショットコピー作成のための素材

### 7.1 スクショ1枚目の最有力訴求

「録音した声を、すぐに変えて楽しめる」

### 7.2 5枚構成案

| 枚数 | 伝える内容 | 画面/素材 | キャッチコピー方向性 |
| --- | --- | --- | --- |
| 1 | 録音してすぐ変声 | Record画面 + Listen画面への導線 | Record your voice. Change it instantly. |
| 2 | 複数の声エフェクト | Listen画面のエフェクトアイコン | Try fun voice effects |
| 3 | 速さとピッチ調整 | Speed/Pitchスライダー表示 | Tune speed and pitch |
| 4 | 録音一覧管理 | Record画面の録音リスト | Keep your recordings |
| 5 | 共有 | Listen画面の共有ボタン/共有シート | Share your sound |

### 7.3 Before/Afterや作例が必要か

- 必要度: 中。
- 理由: 音声アプリは静止画だけでは変化が伝わりにくい。スクショでは「Normal / Fast / Slow / Baby / Scary / Robot / Custom」のような効果名やアイコンにコピーを重ねると伝わりやすい。
- 推奨素材: 録音前/録音後の2画面、エフェクト選択状態、Speed/Pitch調整状態、共有画面。音の違いはApp Preview動画があればより伝わる。

---

## 8. 課金・広告・プレミアム機能の扱い

### 8.1 収益化要素

| 種類 | 有無 | ユーザー向け説明に使うか | 備考 |
| --- | --- | --- | --- |
| 広告 | 有 | 通常は使わない | Google Mobile Ads、バナー、インタースティシャル、App Open広告、リワード広告の実装あり |
| 広告非表示IAP | 有 | 必要なら短く | `black.appextreme.voicechanger.noad`。StoreKit 2で買い切り広告非表示 |
| サブスク | 未確認/なし | No | サブスクリプション実装は確認できない |
| リワード広告 | 有 | メタデータでは控えめに | 動画視聴で30分間広告非表示 |

### 8.2 メタデータで避けるべきこと

- 「広告なしアプリ」と表現しない。無料利用時は広告が出る前提。
- 「完全無料」と表現しない。広告非表示IAPがある。
- リワード動画を主要機能のように見せない。ユーザー価値は録音/変声/共有。
- IAP価格や広告表示頻度はApp Store Connect/実機で未確認のため断定しない。

---

## 9. ローカライズ時の注意

### 9.1 翻訳しないほうがよい語

- `Magic Voice`: ブランド名として維持する方向が安全。
- `NoAds`: UI上の短いバッジ。各言語では短く翻訳されているが、英語マスターでは短さ優先。
- `Speed` / `Pitch`: 音声編集の基本語。英語版ではそのままが自然。

### 9.2 国・言語によって表現を変えるべき内容

- 「prank」は地域や年齢層によって受け取りが変わるため、英語マスターでは強く押しすぎない。
- 広告/リワード/IAPの説明は、誤解を避けるため各言語で「広告を完全に削除」「30分間非表示」の区別を明確にする。
- アラビア語などRTL言語では `%@` や分数表記を壊さない。`localization/no_ads_localization_master.md` でもプレースホルダー維持が明記されている。

### 9.3 英語マスターコピーの方向性

- Tone: short, playful, clear, casual.
- Avoid: AI claims, professional audio claims, unlimited/complete transformation claims, free no-ads claims.

---

## 10. 証拠・根拠メモ

| 判断 | 根拠 |
| --- | --- |
| アプリ名はMagic Voice | `Info.plist` / `CFBundleName`、`InfoPlist.strings` / `CFBundleDisplayName` |
| Bundle IDは`black.appextreme.voicechanger` | `project.pbxproj` / `PRODUCT_BUNDLE_IDENTIFIER` |
| カテゴリはUtilities | `project.pbxproj` / `INFOPLIST_KEY_LSApplicationCategoryType` |
| マイク録音アプリ | `Info.plist` / `NSMicrophoneUsageDescription`、`ViewController.recordAct(_:)` |
| 録音はアプリ内Libraryの`sounds`へ保存 | `sGlobal.swift` / `libPath`、`ViewController.recordAct(_:)` |
| 録音一覧がある | `Main1.storyboard` / `UITableView`、`recordsCell.swift`、`ViewController.tableView` |
| 録音停止後にListen画面へ遷移 | `ViewController.stopAct(_:)` / `performSegue(withIdentifier: "goListen")` |
| 再生/変声画面がある | `Main1.storyboard` / Listen scene、`listenView` |
| 速い/遅い再生がある | `listenView.playSound(_:)` / `audioP.rate = 1.5`、`audioP.rate = 0.5` |
| ピッチ変更がある | `listenView.playAudioWithVariablePitch(_:)` / `AVAudioUnitTimePitch` |
| カスタム調整がある | `listenView.customAct(_:)`、`speedSlider`、`pitchSlider` |
| 共有がある | `listenView.shareApp(_:)` / `UIActivityViewController` |
| 録音名変更/削除がある | `ViewController.tableView(_:editActionsForRowAt:)` / `Rename`、`Delete` |
| 広告がある | `Info.plist` / `GADApplicationIdentifier`、`Config.plist` / AdMob ID群、`AppDelegate.swift`、`sGlobal.swift` |
| 広告非表示IAPがある | `Config.plist` / `removeAdsInAppPurchaseId`、`AdRemovalManager.purchaseRemoveAds()` |
| リワード動画で30分広告非表示 | `AdRemovalManager.rewardedDuration = 30 * 60`、`Localizable.strings` / `Hide for 30 min` |
| 旧アプリ概要は録音・変声・保存・共有を訴求 | ユーザー提供の旧アプリ概要欄テキスト |
| 旧スクショは録音画面/Listen画面/広告枠/共有ボタンを示す | `MagicVoice_Old/1.MagicVoice/images/*.png` の7枚を確認 |

---

## 11. 未確認・手動確認が必要なこと

- Xcodeで現在のStoryboardが正常に表示されるか。
- App Store Connect上のIAP `black.appextreme.voicechanger.noad` が有効か、価格はいくらか。
- 実機でマイク権限拒否時の表示が十分か。
- 広告同意フォーム、Privacy Options、App Open広告が対象地域で正しく出るか。
- 共有時に、変声後の音ではなく元録音ファイルを共有している可能性がある。`shareApp(_:)` は `selectedPath` のファイルURL共有であり、エフェクト適用後の書き出し保存は確認できない。
- `MessageUI`、`shareText` は存在するが、メール本文共有としては使われていないように見える。
- `upgrade.swift` と `MyPaymentTransactionObserver.swift` は旧StoreKit系の可能性があり、現行導線かは未確認。現行No Adsは `NoAdsViewController` / `AdRemovalManager` が中心に見える。
- 旧スクリーンショットに広告が表示されているため、App Storeスクショ作成時は広告枠をどう扱うか要確認。

---

## 12. ChatGPTへ渡すときの要約

以下をChatGPTに渡すと、App Store用メタデータを作成しやすい。

```text
アプリ名: Magic Voice
カテゴリ: Utilities / ボイスレコーダー / ボイスチェンジャー
一言説明: 録音した声を、スピードやピッチ、複数のエフェクトで手軽に変えて再生・共有できるボイスレコーダーアプリ。
主なユーザー: 友だちや家族と音声で遊びたい人、短い音声を共有したい人、シンプルな録音メモを残したい人。
主な機能: ワンタップ録音、録音一覧、通常/速い/遅い/高い/低い/カスタムの変声再生、Speed/Pitchスライダー、録音ファイル共有、録音名変更、削除。
最重要訴求: 録音してすぐ声を変えられる。速さとピッチを調整して自分だけの声遊びができる。
スクショで見せるべき内容: 録音画面、録音一覧、Listen画面のエフェクトアイコン、Speed/Pitchスライダー、共有ボタン。
キーワード素材: voice changer, voice recorder, audio recorder, sound recorder, sound effects, pitch, speed, playback, share, funny voice, magic voice
避けるべき訴求: AI、プロ品質、完全無料、広告なし、声を完全に別人にする、エフェクト適用後ファイルを書き出して共有できるという断定。
課金/広告注意: 広告あり。買い切りIAPで広告を永久非表示、リワード動画で30分非表示。サブスクは確認できない。
```
