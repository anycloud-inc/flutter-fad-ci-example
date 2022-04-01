# flutter_fad_ci

Example of Continuous Delivery with Flutter, Firebase App Distribution, and Github Actions.

以下の手順で自分のプロジェクトでも使えるようになります。

## 下準備

1. Apple Developer Program に参加

- 基本的にはチームに招待してもらう

2. Firebase の登録

- https://console.firebase.google.com/u/9/?hl=ja から登録
- 登録時に google-service.json などの話など出てくるが、Firebase App Distribution を使うだけであれば無視しても大丈夫

3. Firebase App Distribution の登録

- iOS アプリ, Android アプリの作成
- テストグループを作成
  - CI で testers というグループに配信するようになっているので、testers というグループ名にする必要あり
  - テスターをグループに追加

3. .github/workflows を自分のプロジェクトにコピペ

4. ios/ExportOptions.plist を自分のプロジェクトにコピペ

- teamID は Apple Developer のチーム ID
- provisioningProfiles の部分は 下の IOS_PROVISIONING_PROFILE_BASE64 を作る過程で作成したものに変更
  - key が プロビジョニングプロファイルの App ID
  - string が プロビジョニングプロファイル の Name

## Github の Secrets を登録

- settings / secrets / actions から登録

以下設定項目

### FIREBASE_TOKEN

`firebase login:ci` を叩いて取得

### IOS_FIREBASE_APP_ID

firebase の iOS アプリの ID

<img src="./docs/firebase_app_id.png" />

### IOS_CERTIFICATE_BASE64

1. Xcode の Preferences / Accounts / Manage Certificates まで移動
2. \+ ボタンから `Apple Distribution` を選択
3. 作成された Certificate を右クリックし、 `Export Certificate`
4. 任意のパスワードを入力して Export
5. `base64 XXX.p12 | pbcopy` でクリップボードにコピーし Secrets に貼り付け

### IOS_CERTIFICATE_PASSWORD

IOS_CERTIFICATE_BASE64 を生成する際の Export 時のパスワード

### IOS_PROVISIONING_PROFILE_BASE64

1. https://developer.apple.com/account/resources/profiles/list からプロビジョニングプロファイルを作成

- type は Ad hoc
- Devices にテスターのデバイスを登録
- Certificates は IOS_CERTIFICATE_BASE64 生成時のもの

2. ファイルをダウンロード
3. `base64 XXX.movileprovision | pbcopy` でクリップボードにコピーし Secrets に貼り付け
