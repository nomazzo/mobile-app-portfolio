 # Google Play Release

## 概要

このドキュメントは、AndroidアプリのGoogle Play公開・運用に関する経験を整理したケーススタディです。
実装そのものではなく、Google Play公開に必要な周辺作業を中心に整理します。

主な対象領域は以下です。

* Android Studioでのビルド
* Android App Bundle作成
* アプリ署名
* Google Play Console設定
* ストア掲載情報作成
* プライバシーポリシー整備
* Data safety / データ安全性に関する確認
* 広告利用の申告
* Google Mobile Ads SDK / UMP対応
* スクリーンショット制作
* Google Play向けクリエイティブ制作
* 内部テスト / 本番リリース
* バージョンアップ運用

---

## このケーススタディの位置づけ

Android開発経験は、iOS開発経験と比べると多くありませんが、以下を一通り経験しています。

* 既存iOSアプリの分析
* Android版仕様への変換
* Kotlin / Jetpack Composeによる実装
* Android標準の写真選択・保存・共有導線への置き換え
* Google Mobile Ads SDK導入
* UMP同意管理
* Google Play向けストア情報作成
* スクリーンショット制作
* Google Play Consoleでの公開作業

---

## Androidリリース経験

### リリース経験の概要

| 項目            | 内容                                    |
| ------------- | ------------------------------------- |
| Androidリリース本数 | 2本                                    |
| 最近の主な事例       | iOSフォトエディターアプリをもとにしたAndroid版          |
| 主なアプリ領域       | フォトエディター / 写真加工                       |
| 最近の技術スタック     | Kotlin / Jetpack Compose / Material 3 |
| ストア           | Google Play                           |
| 収益化           | 広告モデル                                 |
| 課金            | 最近のAndroid版ではIAPなし                    |
| 公開方針          | 商用コード・本番ID・ストアURLは公開リポジトリに掲載しない       |

---

## iOS版からAndroid版への展開

最近のAndroid版では、既存iOSアプリの機能とユーザー価値をもとに、Android向けに新規設計しました。

### Android版で置き換えたもの

| 領域     | iOS版                           | Android版                     |
| ------ | ------------------------------ | ---------------------------- |
| UI     | UIKit / Storyboard             | Jetpack Compose / Material 3 |
| 言語     | Objective-C                    | Kotlin                       |
| 写真選択   | Photo Library / UIImagePicker系 | Android Photo Picker         |
| カメラ    | iOSカメラ導線                       | CameraX                      |
| 保存     | Photo Library                  | MediaStore                   |
| 共有     | UIActivityViewController       | Android Sharesheet           |
| 広告     | iOS向けAdMob実装                   | Google Mobile Ads SDK        |
| 同意管理   | iOS向けATT / UMP等                | UMP SDK                      |
| レビュー依頼 | iOS向けレビュー導線                    | Play In-App Review           |

このように、iOS版のコードをそのまま移植するのではなく、Androidで自然に動く構成へ置き換えました。

---

## Google Play公開で対応した主な作業

### Android Studio / ビルド

Android Studioを使って、プロジェクト作成、ビルド、実機確認を行いました。

対応経験：

* Android Studioでのプロジェクト作成
* Gradle設定確認
* Kotlin / Jetpack Compose構成
* minSdk / targetSdk / compileSdk設定
* namespace / applicationId設定
* Debug / Releaseビルド確認
* 実機・エミュレータでの動作確認
* 画像選択・保存・共有の動作確認
* 広告SDK導入後のビルド確認
* リリース前のクラッシュ確認

---

### Android App Bundle作成

対応経験：

* Releaseビルド作成
* Android App Bundle作成
* versionCode / versionName管理
* ProGuard / minify設定確認
* リリースビルドでの動作確認
* Google Play Consoleへのアップロード
* 署名関連エラーの確認
* アップロード後の警告確認

公開リポジトリでは、実際の署名情報やkeystoreは掲載しません。

---

### アプリ署名 / keystore管理

対応経験：

* keystore作成
* release signing設定
* Play App Signingへの対応
* keystoreファイルの非公開管理
* key alias / passwordの管理
* Gitにkeystoreや秘密情報を含めない設定
* `.gitignore`による除外確認
* リリースビルド時の署名確認

---

## Google Play Console設定

主な対応経験：

* 新規アプリ作成
* アプリ名設定
* デフォルト言語設定
* アプリ / ゲーム種別設定
* 無料 / 有料設定
* カテゴリ設定
* 連絡先情報設定
* Privacy Policy URL設定
* ストア掲載情報入力
* スクリーンショット登録
* アイコン登録
* Feature Graphic登録
* AABアップロード
* リリースノート作成
* テストリリース
* 本番リリース

---

## ストア掲載情報作成

対応経験：

* アプリ名
* 短い説明
* 詳細説明
* スクリーンショット
* アイコン
* Feature Graphic
* アプリカテゴリ
* タグ
* Privacy Policy
* 連絡先情報
* リリースノート

---

## プライバシーポリシー / データ安全性

対応経験：

* Privacy Policy URL設定
* アプリ内または設定画面からのPrivacy Policy導線
* 広告利用に関する説明
* カメラ権限の利用目的整理
* 写真選択の利用目的整理
* 画像保存・共有の動作説明
* データ安全性に関する項目確認
* ユーザーデータを外部送信しない機能の整理
* 広告SDK利用時の扱い整理
* UMP同意管理との整合確認

---

## 広告 / Google Mobile Ads

対応経験：

* Google Mobile Ads SDK導入
* バナー広告
* Adaptive Banner
* 保存後などのインタースティシャル広告候補
* 広告ロード状態管理
* 広告ロード失敗時のUI調整
* テスト広告での動作確認
* 本番広告IDをGitに含めない管理
* 広告表示位置の調整
* 広告と編集体験のバランス調整

---

## UMP / 同意管理

対応経験：

* UMP SDK導入方針の整理
* 広告表示前の同意状態確認
* Privacy Options導線の検討
* 設定画面からプライバシー関連項目へアクセスする設計
* 広告SDKと同意状態の連携
* 公開前の同意フロー確認

---

## Play In-App Review

対応経験：

* Play In-App Review導入方針
* 保存成功後など自然なタイミングでのレビュー依頼
* 保存回数に応じた表示制御
* 過度にレビュー依頼を出しすぎない条件設計
* レビュー依頼に失敗してもアプリ体験を壊さない設計

フォトエディターアプリでは、保存に成功したタイミングなど、ユーザーが成果物を得た直後にレビュー依頼を検討します。

---

## テストリリース / 本番リリース

対応経験：

* 内部テスト
* 本番リリース
* リリースノート作成
* AABアップロード
* バージョンコード確認
* 実機テスト
* 画像選択確認
* カメラ撮影確認
* 画像保存確認
* 画像共有確認
* 広告表示確認
* 権限拒否時の動作確認
* クラッシュ確認
* ストア掲載情報との整合性確認

---

## iOS版とAndroid版の違いを整理

最近のAndroid版では、iOSアプリの機能をそのまま移行するのではなく、Android版として自然に成立するように整理しました。

具体的には、以下のような判断を行いました。

* iOSのStoryboard構造は引き継がない
* Objective-Cコードは直接移植しない
* AndroidではPhoto Pickerを使う
* AndroidではMediaStoreで保存する
* 共有はAndroid Sharesheetへ置き換える
* iOSの広告削除IAPはAndroid初期版では入れない
* 編集機能はすべてではなく、MVPとして成立する範囲に絞る
* Androidのストア要件や権限要件に合わせる
* Google Play向けのストア素材を作成する

---

## 関連リポジトリ

Google Play公開・運用経験に関連する公開用リポジトリは以下です。

| Repository                    | 目的                                                |
| ----------------------------- | ------------------------------------------------- |
| [android-photo-editor-sample](https://github.com/nomazzo/android-photo-editor-sample) | Kotlin / Jetpack Composeで再構成したAndroid写真編集アプリのサンプル |
| [ios-photo-editor-sample](https://github.com/nomazzo/ios-photo-editor-sample) | iOS版ペインティング風カメラ・リアルタイムフィルター・保存共有フローのサンプル                             |
| [mobile-app-portfolio](https://github.com/nomazzo/mobile-app-portfolio) | 全体のアプリポートフォリオ説明 |

