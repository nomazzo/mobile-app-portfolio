# Android Photo Editor App Case Study

## 概要

Android向けフォトエディターアプリの開発・リリース経験を整理します。

このアプリは、既存のiOSフォトエディターアプリを参考にしつつ、Android版として新規に設計・実装したものです。
iOS版のコード、Storyboard、Objective-C構造、既存ライブラリをそのまま移植するのではなく、ユーザーが得ていた価値、主要フロー、編集体験、広告表示、保存・共有導線をAndroidネイティブの設計へ置き換えています。

このケーススタディでは、商用アプリ本体のコードやGoogle Play URLを公開するのではなく、Android版を作成する際の設計方針、技術選定、実装経験、公開用サンプルへ再構成できる内容を整理します。

商用アプリ本体のソースコード、実際のpackage name、AdMob本番ID、keystore、非公開素材、Google Play URLは掲載しません。必要に応じて、実際のストア実績やURLはエージェント・発注企業向けに個別共有します。

---

## このケーススタディの位置づけ

このケーススタディは、以下を示すためのものです。

* iOSアプリの価値をAndroidアプリとして再設計できること
* iOSコードの単純移植ではなく、AndroidネイティブのUX・技術へ置き換えられること
* Kotlin / Jetpack ComposeによるAndroidアプリ開発経験
* CameraX、Photo Picker、MediaStore、SharesheetなどAndroid標準機能の利用経験
* Google Mobile Ads SDK、UMP、Play In-App Reviewなど、Google Play公開前提の実装経験
* 画像編集アプリに必要な入力、編集、保存、共有、広告表示の一連のフロー設計経験

iOS版の価値を参考にしながらも、Androidユーザーに自然な操作感になるように再構成した点が、この事例の中心です。

---

## 対象アプリ

対象は、食べ物写真向けのフォトエディターアプリです。

ユーザーが写真を撮影する、または端末内の写真を選択し、数タップで明るく、おいしそうに加工し、保存・共有できることを目的にしています。

主な価値：

* 食べ物写真を見栄えよく加工できる
* 写真を撮る、または選んで編集できる
* 数タップでフィルターや補正を適用できる
* 保存・共有まで迷わず完了できる
* Androidネイティブの自然な操作感で使える
* iOS版の古い構造やライブラリに依存しない

---

## iOS版からAndroid版への再設計方針

このアプリでは、iOS版の構造をそのまま移植するのではなく、以下のようにAndroid向けへ再設計しました。

| 領域     | iOS版からの扱い                      | Android版での方針                               |
| ------ | ------------------------------ | ------------------------------------------ |
| UI構造   | Storyboard / UIKit系の構成を直接移植しない | Kotlin + Jetpack Compose + Material 3で新規構成 |
| 画像編集   | iOS版の編集機能を解析                   | Androidで実装可能な機能に分解して再設計                    |
| 画像選択   | iOSのPhoto Library導線            | Android Photo Pickerへ置き換え                  |
| カメラ    | iOSのカメラ実装                      | CameraXまたはAndroid向け撮影フローへ置き換え              |
| 保存     | iOSの写真ライブラリ保存                  | MediaStoreで端末のPictures配下へ保存                |
| 共有     | iOSの共有導線                       | Android Sharesheetへ置き換え                    |
| 広告     | iOS側の広告/IAP構成をそのまま使わない         | Google Mobile Ads SDK + UMPを利用             |
| 課金     | iOS版のIAP/広告削除導線は引き継がない         | Android初期版ではIAPなし、広告常時表示方針                 |
| レビュー依頼 | iOS向け導線をそのまま移植しない              | Play In-App Reviewを自然なタイミングで利用             |

この方針により、iOS版の体験を参考にしながらも、Androidアプリとして自然で保守しやすい構成を目指しました。

---

## 対象ユーザー

主な想定ユーザーは以下です。

| ユーザー                         | ニーズ                       | アプリが提供する価値                  |
| ---------------------------- | ------------------------- | --------------------------- |
| 食べ物写真をよく撮るユーザー               | 料理写真を明るく、おいしそうに見せたい       | 食べ物向け補正・フィルターで簡単に加工できる      |
| SNS投稿向けに写真を整えたいユーザー          | 撮った写真をすぐ保存・共有したい          | 撮影/選択から保存/共有まで短い導線で完了できる    |
| 難しい編集アプリが苦手なユーザー             | 複雑な操作なしで見栄えを整えたい          | ワンタップ補正や簡易編集で扱いやすい          |
| 既存iOS版に近い体験をAndroidで使いたいユーザー | Android端末でも同じような編集体験を使いたい | Android向けに再構成された自然なUIで利用できる |

---

## 主なユーザー価値

このAndroid版で提供する主な価値は以下です。

* 写真を撮影してすぐ編集できる
* 端末内の写真を選んで編集できる
* 食べ物写真を明るく、おいしそうに見せられる
* 保存・共有まで迷わず進める
* Android標準のPhoto Pickerを使い、過剰な権限要求を避けられる
* MediaStoreを使ってAndroidらしい保存導線を提供できる
* 編集中の操作を邪魔しない広告配置を設計できる
* iOS版の価値を保ちつつ、Androidに自然な体験へ置き換えられる

---

## 主な画面構成

### Main Screen

写真を取り込み、編集・保存・共有へ進む中心画面です。

主な要素：

* 編集対象画像プレビュー
* 起動直後のデフォルト画像
* 画像に合わせた背景
* Camera / Photos / Edit / Save / Settings相当の下部メニュー
* バナー広告枠
* ローディング表示

Main Screenでは、起動直後から完成状態に近い見た目を出し、カメラ撮影または写真選択からすぐ編集へ進める短い導線を重視します。

---

### Camera Screen

アプリ内で写真を撮影し、編集対象として取り込む画面です。

主な要素：

* カメラプレビュー
* 撮影ボタン
* 戻るボタン
* カメラ切替
* フラッシュ切替

撮影成功後は、撮影画像をアプリ内部キャッシュに保存し、現在画像としてMain ScreenまたはEditor Screenへ渡します。
ユーザーが明示的にSaveを押すまでは、端末の写真ライブラリには保存しない設計です。

---

### Photo Picker

端末内の画像から編集対象を選ぶために、Android Photo Pickerを利用します。

主な設計方針：

* 画像のみ単一選択
* キャンセル時は現在画像を変更しない
* 広範な写真権限を避ける
* 選択画像を現在画像として反映する
* 必要に応じてアプリ内部キャッシュへコピーする

---

### Editor Screen

選択または撮影した画像に編集を加える画面です。

主な要素：

* 編集対象画像
* 戻る / 適用 / リセット
* 下部編集ツール
* スライダー
* プリセット選択
* テキストやステッカーなどの編集UI

Editor Screenでは、編集中の誤タップや体験阻害を避けるため、広告表示は行わない方針です。

---

### Save / Share

編集後の画像を保存・共有する導線です。

保存：

* MediaStoreを使って端末のPictures配下へ保存
* 保存成功時に結果を表示
* 保存回数に応じてレビュー依頼を検討

共有：

* Android Sharesheetを利用
* `image/jpeg` として共有
* 他アプリへ自然に渡せるようにする

---

### Settings / Info Screen

アプリ情報やプライバシー関連導線を置く画面です。

主な要素：

* Privacy Policy
* Terms
* App version
* Support
* 広告・プライバシー設定
* UMP関連導線

Android版では、初期方針としてIAPや広告削除購入を採用しないため、購入・復元・広告削除項目は置かない設計です。

---

## 採用技術

主な採用技術は以下です。

| 領域     | 技術                            |
| ------ | ----------------------------- |
| 言語     | Kotlin                        |
| UI     | Jetpack Compose               |
| デザイン   | Material 3                    |
| 画面遷移   | Navigation Compose            |
| 画像選択   | Android Photo Picker          |
| カメラ    | CameraX                       |
| 保存     | MediaStore                    |
| 共有     | Android Sharesheet            |
| 設定保存   | SharedPreferences / DataStore |
| 広告     | Google Mobile Ads SDK         |
| 同意管理   | Google UMP SDK                |
| レビュー依頼 | Play In-App Review            |
| IDE    | Android Studio                |
| 開発補助   | VS Code / Codex / Claude Code |

---

## 実装しないもの

Android初期版では、iOS版のすべてを再現するのではなく、Android版として必要な範囲に絞ります。

初期版で実装しないもの：

* IAP
* サブスクリプション
* 広告削除購入
* 購入復元
* 購入済み広告非表示状態
* Upgrade画面
* リワード広告
* AppLovinなどの旧SDK
* iOS版の全フィルター完全再現
* iOS版の最近写真サムネイル独自パネル
* Exif / GPS情報の完全引き継ぎ
* iOSのStoryboard / Objective-C構造の直接移植
* iOS版CLImageEditor内部構造の直接移植

このようにスコープを絞ることで、まずは写真入力、編集、保存、共有という本質的な体験を優先しました。

---

## 主な機能経験

### 入力・撮影

* Android Photo Picker
* CameraX
* 画像URIの取得
* 画像読み込み
* 画像の向き補正
* 大きすぎる画像の縮小
* アプリ内部キャッシュ
* カメラ権限
* カメラ利用不可時の代替導線

---

### プレビュー・状態管理

* 現在画像URI
* 編集済みBitmap
* 元画像URI
* 編集パラメータ
* 保存処理中フラグ
* 広告ロード状態
* UMP同意状態
* 保存回数
* 最終レビュー依頼日時
* 広告表示制御に必要な状態
* SharedPreferences / DataStoreによる永続化

---

### 編集機能

* 画像プレビュー
* Crop
* Rotate
* Adjust
* Enhance
* Filter
* Effect
* Blur
* Text
* Sticker
* Kaleidoscope
* Gradient
* Draw
* Retouch
* Tone Curve

---

### 保存・共有

* MediaStore保存
* 保存先ディレクトリ設計
* 保存成功通知
* Android Sharesheet
* `image/jpeg`共有
* 編集済み画像の一時ファイル管理
* 保存/共有対象への編集結果反映

---

### 広告・レビュー

* Google Mobile Ads SDK
* バナー広告枠
* Adaptive Banner
* 広告ロード状態管理
* 広告ロード失敗時のレイアウト安定化
* UMP同意管理
* Play In-App Review
* 保存成功後など自然なレビュー依頼タイミング

---

## 画像編集機能の設計

### Enhance

食べ物写真向けの補正プリセットを扱う機能です。

主な設計：

* Food
* Scenery
* Night
* Portrait
* HighDef
* 効果量スライダー
* プレビューへの即時反映
* 保存/共有画像への反映

iOS版に存在する自動補正や赤目補正などは、Android初期版では工数や食べ物写真向け価値を考慮して外す方針です。

---

### Filter

ワンタップで写真の色味や雰囲気を変える機能です。

主な設計：

* 既存フィルター
* iOS版由来のLUTフィルター
* サムネイル付きフィルター一覧
* フィルター強度スライダー
* LUT適用処理
* プレビュー用Bitmapと保存用Bitmapの分離

iOSのGPUImage実装を直接移植するのではなく、Android側で汎用LUT適用処理を新規実装する方針です。

---

### Effect

写真にアート風・演出系の効果を加える機能です。

主な対象：

* Sepia
* Pixellate
* Posterize
* Halftone
* Cross Hatch
* Black / White
* Edge
* Sketch
* Toon
* Sharpen
* Bloom
* Haze

幾何変形や特殊演出は、プレビューUIや処理負荷が大きいため、初期版では必要なものに絞ります。

---

### Blur

ぼかしとフォーカスを扱う機能です。

主な対象：

* Normal Blur
* Circle Focus
* Band Focus
* ぼかし強度スライダー
* フォーカス領域ガイド
* 中心移動
* 半径 / 帯幅調整
* 角度調整

保存/共有画像にも同じぼかし結果が反映されるよう、プレビュー用と出力用の処理を分けます。

---

### Text

写真上に文字を配置する機能です。

主な対象：

* 複数テキスト
* 本文編集
* 文字色
* アウトライン色
* アウトライン幅
* フォント選択
* 左 / 中央 / 右揃え
* 移動
* 拡大縮小
* 回転
* 左右反転
* 削除

保存/共有時には、編集ハンドルやガイドを含めず、テキストだけをBitmapへ焼き込みます。

---

### Sticker

写真にステッカーを配置する機能です。

主な対象：

* ステッカーカテゴリ
* ステッカー一覧
* 複数StickerLayer
* 移動
* 拡大縮小
* 回転
* 透明度調整
* 左右反転
* 削除
* 追加順によるレイヤー管理

iOS版のPNG素材を活かせる場合でも、Androidでは`assets`配下へ安全に配置し、Canvas描画で保存/共有画像へ反映する方針です。

---

### Draw

写真上へ自由に描画する機能です。

主な対象：

* 指描画
* 色選択
* 線幅調整
* 消しゴム
* 保存/共有画像への焼き込み

---

### Retouch

円形ブラシでなぞった部分だけを補正する機能です。

主な対象：

* Blur
* Sharpen
* 明るさ補正
* 彩度補正
* Warm / Cold
* Pixel系補正
* ブラシサイズ
* ブラシガイド

---

### Tone Curve

画像全体の明暗階調をカーブで調整する機能です。

主な対象：

* 5点カーブ
* グリッド表示
* 制御点ドラッグ
* リセット
* RGB共通カーブ

初期版ではRGB個別チャンネルではなく、共通カーブを対象にします。

---

## Android版での実装上の工夫

### Androidユーザーに自然なUIへ置き換える

iOS版の画面やStoryboardをそのまま再現するのではなく、Androidユーザーにとって自然な操作感になるように、Compose + Material 3で再構成しました。

意識した点：

* Android標準の操作感に寄せる
* 権限要求を最小限にする
* 画像選択はAndroid Photo Pickerを利用する
* 保存はMediaStoreを使う
* 共有はAndroid Sharesheetを使う
* 下部メニューやツール選択をAndroid向けに整理する

---

### iOS版の価値を機能単位で分解する

iOS版にある機能をそのまま移植するのではなく、機能ごとに以下を判断しました。

* Android初期版に入れるもの
* 後続候補に回すもの
* 実装しないもの
* Androidでは別の方法で実現するもの

この判断により、初期版では写真入力、編集、保存、共有の本質を優先し、重い特殊演出やiOS固有の機能は後回しにしています。

---

### 編集中の広告表示を避ける

写真編集画面では、広告による誤タップや体験阻害を避けるため、Editor Screenにはバナー広告を置かない方針です。

一方で、Main Screenにはバナー広告枠を確保し、広告ロード失敗時も画面全体の縦位置が大きく跳ねないようにします。

これにより、無料アプリとしての収益化と編集体験のバランスを取ります。

---

### プレビュー用Bitmapと出力用Bitmapを分ける

画像編集アプリでは、大きな画像をそのままリアルタイム処理すると動作が重くなりやすくなります。

そのため、以下のように分けて扱います。

* プレビュー用：縮小Bitmapで高速に表示
* 保存/共有用：出力用Bitmapへ同じ編集結果を反映

この設計により、プレビュー操作の軽さと保存画像の品質の両方を考慮します。

---

### iOS由来の素材・機能を安全に扱う

iOS版のLUTやPNG素材を活かせる場合でも、Androidリソース名、配置場所、ファイル名制約、ライセンス、商用アセットの扱いに注意します。

公開用サンプルでは、商用素材や非公開素材は含めません。
必要に応じて、サンプル用の安全な画像・LUT・ステッカーへ置き換えます。

---

### Google Play公開前提の設計

Android版では、単にローカルで動くアプリではなく、Google Play公開を前提に以下を考慮します。

* package name / namespace管理
* minSdk / targetSdk / compileSdk
* App Bundle
* keystore管理
* Privacy Policy
* 広告利用の申告
* UMP同意管理
* Data Safety
* 実機テスト
* 保存・共有の動作確認
* 権限説明
* バージョンコード管理

---

## App / Play Store運用での経験

このAndroid版では、実装だけでなく、Google Play公開を前提とした設計・運用も扱います。

主な経験：

* Android Studioでのビルド
* Kotlin / Composeプロジェクト構成
* App Bundle作成
* Google Play Console登録
* アプリ署名
* ストア掲載情報作成
* プライバシーポリシー整備
* Google Mobile Ads SDK導入
* UMP同意管理
* 保存・共有など権限関連の確認
* 実機テスト
* Google Play向けスクリーンショット・説明文作成
* バージョンアップ対応

---

## 公開用サンプルコード方針

このケーススタディに対応する公開用コードは、`android-photo-editor-sample` として別リポジトリで作成する方針です。

商用アプリ本体のコードは含めず、Androidフォトエディターに必要な主要機能に絞って、公開可能な形に再構成します。

---

## 公開用サンプルで扱う予定の機能

公開用サンプルでは、以下を中心に実装します。

* Kotlin
* Jetpack Compose
* Material 3
* Android Photo Picker
* 画像プレビュー
* 簡易フィルター
* 明るさ・コントラスト・彩度調整
* Crop / Rotate
* Save to MediaStore
* Android Sharesheet
* Settings / Info画面
* 広告表示枠のサンプル
* UMP同意管理の設計メモ
* Play In-App Reviewの設計メモ
* テストしやすい状態管理

必要に応じて、次の機能も段階的に追加します。

* CameraX
* Text
* Sticker
* Blur
* Enhance
* LUT Filter
* Draw
* Tone Curve

---

## 公開用サンプルで扱わないもの

公開用サンプルでは、以下は含めません。

* 商用アプリ本体のソースコード
* 実際のpackage name
* AdMob本番ID
* keystore
* Google Play URL
* 商用アセット
* 非公開素材
* 実際の収益情報
* Google Play Console内部画面
* すべてのフィルターやステッカーの完全再現
* iOS版由来の商用素材
* AppLovinなど採用しない旧SDK
* 本番運用設定

---

## 関連予定リポジトリ

このケーススタディに関連する公開用サンプルとして、以下のリポジトリを予定しています。

| Repository                    | 目的                                                |
| ----------------------------- | ------------------------------------------------- |
| `android-photo-editor-sample` | Kotlin / Jetpack Composeで再構成したAndroid写真編集アプリのサンプル |
| `mobile-app-production-kit`   | 広告、レビュー依頼、ローカライズ、設定画面などの実装サンプル                    |
| `ios-photo-editor-sample`     | iOS版の写真編集・フィルター・保存共有フローのサンプル                      |

---

## 掲載しないもの

このケーススタディでは、以下の情報は掲載しません。

* 商用アプリ本体のソースコード
* 実際のpackage name
* AdMob本番ID
* keystore
* APIキー
* 非公開素材
* Google Play URL
* 収益情報
* Google Play Console内部画面
* 審査対応メッセージ
* ユーザーデータ
* 商用アプリ固有の内部設定

---

## まとめ

このAndroidフォトエディターアプリは、iOS版アプリの価値をもとにしながら、Android向けに新規設計・実装した事例です。

単純なコード移植ではなく、以下の点を重視しました。

* iOS版の価値をAndroidネイティブUXへ置き換える
* Kotlin / Jetpack Composeで再構成する
* Android Photo Picker / CameraX / MediaStore / Sharesheetを利用する
* 編集、保存、共有までの短い導線を作る
* Google Mobile Ads SDK / UMP / Play In-App Reviewなど、Google Play公開前提の要素を考慮する
* IAPや旧SDKは引き継がず、Android初期版として必要な範囲に絞る
* 公開用サンプルでは商用コードや本番IDを含めず、主要機能だけを安全に再構成する

この事例により、iOSアプリ開発だけでなく、既存iOSアプリの価値をAndroidへ展開する設計・実装経験も示せます。