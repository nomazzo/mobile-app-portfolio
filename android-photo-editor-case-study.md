# Android Photo Editor App Case Study

## 概要

このドキュメントは、Android向けフォトエディターアプリの開発・リリース経験を整理したケーススタディです。

Androidアプリのリリース経験は、現時点では3本です。

このケーススタディでは、比較的新しいAndroidリリース事例として、iOS版フォトエディターアプリをもとにAndroid版を新規設計・実装した経験を整理します。

このAndroid版は、iOS版のソースコードをそのまま移植したものではありません。
既存iOSアプリのユーザー体験、主要機能、編集フロー、保存・共有導線、広告タイミングなどを分析し、Androidネイティブの技術・UI・ストア運用に合わせて再設計しました。

このリポジトリでは、商用アプリ本体のソースコード、実際のpackage name、AdMob本番ID、keystore、非公開素材、Google Play URLは掲載しません。実際のストアURLや詳細な実績情報は、必要に応じて個別共有します。

---

## スクリーンショット

| en (1) | en (2) | en (4) |
|---|---|---|
| <img src="./docs/screenshots/android-photo-editor/en%20%281%29.png" width="220" alt="en (1)"> | <img src="./docs/screenshots/android-photo-editor/en%20%282%29.png" width="220" alt="en (2)"> | <img src="./docs/screenshots/android-photo-editor/en%20%284%29.png" width="220" alt="en (4)"> |

スクリーンショットは `docs/screenshots/android-photo-editor/` に配置しています。追加の画面例やソースコードは、個別リポジトリ `android-photo-editor-sample` に配置しています。

---

## このケーススタディの位置づけ

このケーススタディは、以下を示すためのものです。

* iOSアプリの価値をAndroidアプリとして再設計できること
* iOSコードの単純移植ではなく、AndroidネイティブのUI / UX / 技術へ置き換えられること
* Kotlin / Jetpack ComposeによるAndroidアプリ開発経験
* CameraX、Android Photo Picker、MediaStore、Android Sharesheetなどの標準機能の利用経験
* Google Mobile Ads SDK、UMP、Play In-App Reviewなど、Google Play公開前提の実装経験
* 写真編集アプリに必要な入力、編集、保存、共有、広告表示の一連のフロー設計経験
* 既存iOS資産を分析し、Android版として現実的なMVPへ落とし込む設計経験

---

## 対象アプリ

対象は、食べ物写真向けのフォトエディターアプリです。

ユーザーが写真を撮影する、または端末内の写真を選択し、数タップで明るく、おいしそうに加工し、保存・共有できることを目的にしています。

主な価値：

* 食べ物写真を明るく、おいしそうにワンタッチ加工できる
* 写真を撮る、または選んで編集できる
* 数タップでフィルターや補正を適用できる
* 保存・共有まで迷わず完了できる
* Android標準の写真選択・保存・共有導線を使える
* iOS版の価値を保ちつつ、Androidに自然な操作感へ置き換えている

---

## iOS版からAndroid版への再設計

このAndroid版は、iOSアプリを参考にしていますが、直接移植ではありません。

元のiOS版は、Objective-C、UIKit、Storyboard、CLImageEditorなどを中心に構成された既存アプリです。
一方、Android版では、Kotlin、Jetpack Compose、Material 3、CameraX、Android Photo Picker、MediaStore、Android Sharesheetなどを使い、Android向けに新しく設計しています。

### 変換方針

| 領域     | iOS版                                     | Android版                                           |
| ------ | ---------------------------------------- | -------------------------------------------------- |
| UI     | UIKit / Storyboard                       | Jetpack Compose / Material 3                       |
| 言語     | Objective-C                              | Kotlin                                             |
| 画像編集   | CLImageEditor / CoreImage / GPUImage系    | Android独自Editor Screen / Bitmap / Canvas / Shader等 |
| 写真選択   | UIImagePickerController / Photo Library  | Android Photo Picker                               |
| カメラ    | UIImagePickerController Camera           | CameraX                                            |
| 保存     | Photo Library / 共有シート経由                  | MediaStore                                         |
| 共有     | UIActivityViewController                 | Android Sharesheet                                 |
| 広告     | AdMob / App Open / Interstitial / Banner | Google Mobile Ads SDK                              |
| 同意管理   | ATT / UMP系処理                             | UMP SDK                                            |
| レビュー依頼 | SKStoreReviewController                  | Play In-App Review                                 |
| IAP    | 広告削除IAPあり                                | Android初期版では採用しない                                  |

---

## Android版で重視した方針

### 1. iOSコードを直接移植しない

iOS版のObjective-Cコード、Storyboard構造、CLImageEditor内部構造は直接移植しません。

理由：

* iOS固有の構造をAndroidへそのまま持ち込むと保守しにくい
* Androidユーザーにとって自然なUIになりにくい
* CoreImage / GPUImage依存の処理をそのままAndroidへ再現するのはコストが高い
* Android標準の写真選択・保存・共有導線と合わない
* 旧SDKや古いIAP導線を引き継ぐ必要がない

Android版では、iOS版の「ユーザーが得ていた価値」を抽出し、Androidネイティブの構成へ置き換えました。

---

### 2. MVPを現実的な範囲に絞る

iOS版には多くの編集機能がありますが、Android初期版ではすべてを完全再現するのではなく、まず写真入力、基本編集、保存、共有を成立させることを優先しました。

MVPで重視したもの：

* 写真を選択できる
* 写真を撮影できる
* 画像をプレビューできる
* 基本編集を適用できる
* 編集結果を保存できる
* 編集結果を共有できる
* 広告表示の基本導線を持つ
* UMP同意管理の土台を持つ
* Play In-App Reviewの土台を持つ

MVPで優先しないもの：

* IAP
* サブスクリプション
* 広告削除購入
* リワード広告
* App Open Ad
* iOS版の全フィルター完全再現
* iOS版の全編集ツール完全再現
* Exif / GPS情報の完全保持
* 位置情報を使った広告パーソナライズ
* 旧SDKや不要なライブラリの引き継ぎ

---

### 3. Androidユーザーに自然な操作感へ置き換える

iOS版の画面構成をそのまま再現するのではなく、Androidユーザーにとって自然な操作感になるように再構成しています。

主な方針：

* Jetpack Composeで画面を構成する
* Material 3に沿ったUIにする
* Android Photo Pickerで写真選択を行う
* CameraXで撮影フローを構成する
* MediaStoreで保存する
* Android Sharesheetで共有する
* 権限要求をできるだけ軽くする
* 編集画面では広告による誤タップを避ける
* 保存・共有までの導線を短くする

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

Main Screenでは、起動直後から完成状態に近い見た目を出し、ユーザーがすぐにカメラ撮影・写真選択・編集へ進める導線を重視します。

---

### Camera Screen

アプリ内で写真を撮影し、編集対象として取り込む画面です。

主な要素：

* カメラプレビュー
* 撮影ボタン
* 戻るボタン
* カメラ切替
* フラッシュ切替

撮影後は画像をアプリ内部キャッシュへ保存し、現在画像としてMain ScreenまたはEditor Screenへ渡します。
ユーザーが明示的にSaveを押すまでは、端末の写真ライブラリには保存しない設計です。

---

### Photo Picker

端末内の画像から編集対象を選ぶために、Android Photo Pickerを利用します。

主な方針：

* 画像のみ単一選択
* キャンセル時は現在画像を変更しない
* 広範な写真権限を避ける
* 選択画像を現在画像として反映する
* 必要に応じてアプリ内部キャッシュへコピーする

Android Photo Pickerを使うことで、古いような広いストレージ権限を避け、Androidユーザーに自然な写真選択体験を提供できます。

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

Editor Screenでは、編集中の誤タップや体験阻害を避けるため、広告表示を控える方針です。
無料アプリとして広告は必要ですが、写真編集操作そのものを邪魔しない配置にすることを重視しています。

---

### Save / Share

編集後の画像を保存・共有する導線です。

保存：

* MediaStoreを使って端末のPictures配下へ保存
* 保存成功時にメッセージを表示
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

Android初期版では、IAPや広告削除購入を採用しないため、購入・復元・広告削除関連のUIは置かない方針です。

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

## アーキテクチャ方針

小規模MVPとして、最初から過度に複雑なアーキテクチャにはしません。
ただし、後から拡張しやすいように、画面、画像処理、保存、共有、広告、レビュー依頼などの責務を分ける方針です。

想定構成：

```text
app/
  src/main/java/
    app/
      FoodieLensApp.kt
      AppNavGraph.kt
    ui/
      main/
        MainScreen.kt
        MainViewModel.kt
      editor/
        EditorScreen.kt
        EditorViewModel.kt
      camera/
        CameraScreen.kt
      settings/
        SettingsScreen.kt
      components/
      theme/
    data/
      PreferencesRepository.kt
      ImageRepository.kt
    domain/
      EditableImage.kt
      EditOperation.kt
      FilterPreset.kt
    image/
      ImageLoader.kt
      ImageProcessor.kt
      ImageSaver.kt
      ImageSharer.kt
    ads/
      AdsManager.kt
      BannerAdView.kt
      InterstitialAdManager.kt
      ConsentManager.kt
    review/
      ReviewPrompter.kt
```

このように分けることで、以下を整理しやすくします。

* 画面状態
* 編集状態
* 画像読み込み
* 画像処理
* 保存
* 共有
* 広告
* 同意管理
* レビュー依頼

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
* キャンセル時の安全な状態維持

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
* SharedPreferences / DataStoreによる永続化

---

### 編集機能

Android版では、iOS版に含まれる編集機能を分析し、Android版として必要なものを段階的に実装・整理しました。

主な対象：

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

すべてをiOS版と完全一致させるのではなく、Androidで現実的に実装しやすく、価値のあるものを優先します。

---

### 保存・共有

* MediaStore保存
* 保存先ディレクトリ設計
* JPEG出力
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
* 保存後インタースティシャル広告
* 広告ロード状態管理
* 広告ロード失敗時のレイアウト安定化
* UMP同意管理
* Play In-App Review
* 保存成功後など自然なレビュー依頼タイミング

---

## 画像編集機能の設計

### Enhance

食べ物写真向けの補正プリセットを扱う機能です。

主な対象：

* Food
* Scenery
* Night
* Portrait
* HighDef
* 効果量スライダー
* プレビューへの即時反映
* 保存/共有画像への反映

---

### Filter

ワンタップで写真の色味や雰囲気を変える機能です。

主な対象：

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

### iOSの価値を機能単位で分解する

iOS版にある機能をそのまま移植するのではなく、機能ごとに以下を判断しました。

* Android初期版に入れるもの
* 後続候補に回すもの
* 実装しないもの
* Androidでは別の方法で実現するもの

この判断により、初期版では写真入力、編集、保存、共有の本質を優先し、重い特殊演出やiOS固有の機能は後回しにしています。

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

### AIコーディング支援を使った段階実装

今回のAndroid版作成では、Codex / Claude CodeなどのAIコーディング支援を使い、段階的に実装を進める方針を取りました。

意識した点：

* 先に仕様を整理する
* マイルストーン単位でブランチを切る
* 1つの機能ごとに差分を確認する
* ビルド・実機確認はAndroid Studioで行う
* iOSコードを誤って編集しない
* 本番広告IDやkeystoreをGitに含めない
* 1マイルストーンごとにcommitする

---

## Google Play公開・運用での経験

このAndroid版では、実装だけでなく、Google Play公開を前提とした設計・運用も扱います。

主な経験：

* Android Studioでのビルド
* Kotlin / Composeプロジェクト構成
* Android App Bundle作成
* Google Play Console登録
* アプリ署名
* ストア掲載情報作成
* プライバシーポリシー整備
* Google Mobile Ads SDK導入
* UMP同意管理
* 保存・共有など権限関連の確認
* 実機テスト
* Google Play向けスクリーンショット作成
* Google Play向け説明文作成
* バージョンアップ対応

---

## 公開用サンプルコード方針

このケーススタディに対応する公開用コードは、`android-photo-editor-sample` として別リポジトリに配置しています。

商用アプリ本体のコードは含めず、Androidフォトエディターに必要な主要機能に絞って、公開可能な形に再構成します。


## 関連リポジトリ

このケーススタディに関連する公開用リポジトリは以下です。

| Repository                    | 目的                                                |
| ----------------------------- | ------------------------------------------------- |
| `android-photo-editor-sample` | Kotlin / Jetpack Composeで再構成したAndroid写真編集アプリのサンプル |
| `ios-appstore-production-kit` | iOSアプリ公開・運用に必要な広告、IAP、レビュー依頼、ローカライズ、設定画面などの実装サンプル |
| `ios-photo-editor-sample`     | iOS版のペインティング風カメラ・リアルタイムフィルター・保存共有フローのサンプル                      |

---
