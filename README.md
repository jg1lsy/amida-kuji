# あみだくじ 🎋

ブラウザだけで動く、**複数デバイス対応**のあみだくじアプリです。  
参加者はそれぞれ別のスマートフォン・PCから、好きなタイミングで参加できます。

**→ [ライブデモ](https://jg1lsy.github.io/amida-kuji/)**

---

## 機能

- **開設者**がタイトル・参加人数・結果ラベルを設定し、URLを作成
- **参加者**はURLを受け取り、それぞれ別デバイス・別タイミングでエントリー
  - 自分の名前を入力
  - スタート位置（列）を選択
  - 横棒を1本追加（省略可）
- 全員が参加した瞬間、**全デバイスで自動的に結果発表**
- 当たった人には「🎉 おめでとうございます。当たりました！」
- 外れた人には「😢 残念。ハズレました」
- アニメーション付きで全員の経路を表示

---

## セットアップ（Firebase 設定）

複数デバイス対応のために **Firebase Realtime Database**（無料）が必要です。

### 1. Firebase プロジェクトを作成

1. [Firebase コンソール](https://console.firebase.google.com/) を開く
2. 「プロジェクトを作成」→ プロジェクト名を入力（例：`amida-kuji`）
3. Google アナリティクスは任意（OFFでOK）→「プロジェクトを作成」

### 2. Realtime Database を有効化

1. 左メニュー「構築」→「Realtime Database」
2. 「データベースを作成」→ ロケーションを選択（例：`asia-southeast1`）
3. セキュリティルールは **「テストモードで開始」** を選択 → 「有効にする」

### 3. ウェブアプリを追加して設定を取得

1. プロジェクトのトップ「プロジェクトの概要」横の歯車 ⚙️ →「プロジェクトの設定」
2. 「マイアプリ」セクション → `</>` ボタンでウェブアプリを追加
3. アプリ名を入力（例：`amida-kuji-web`）→「アプリを登録」
4. 表示される `firebaseConfig` の内容をコピー

### 4. `index.html` に設定を貼り付け

`index.html` の先頭付近にある `FIREBASE_CONFIG` を書き換えます：

```javascript
const FIREBASE_CONFIG = {
  apiKey:            "AIzaSy...",          // ← コピーした値に置き換え
  authDomain:        "your-app.firebaseapp.com",
  databaseURL:       "https://your-app-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId:         "your-app",
  storageBucket:     "your-app.appspot.com",
  messagingSenderId: "123456789",
  appId:             "1:123456789:web:abc..."
};
```

### 5. GitHub にプッシュ → GitHub Pages で公開

```bash
git add index.html
git commit -m "Add Firebase config"
git push
```

GitHub Pages の URL（例：`https://yourname.github.io/amida-kuji/`）を参加者に共有してください。

---

## 使い方

### 開設者

1. アプリのURLを開く（`?room=...` なし）
2. タイトル・参加人数・結果ラベルを設定 → 「くじを作る」
3. 表示されたURLをコピーして参加者に送る
4. 参加状況がリアルタイムで更新される

### 参加者

1. 受け取ったURLを開く
2. 名前を入力 → スタート位置を選択 → 横棒を追加（任意）→「参加する」
3. 全員が参加するまで待機画面が表示される
4. 全員参加が完了すると自動で結果が発表される

---

## セキュリティについて

テストモードのデータベースルールは **30日後に期限切れ** になります。  
本番利用では以下のルールを設定してください（Firebase コンソール → Realtime Database → ルール）：

```json
{
  "rules": {
    "rooms": {
      "$roomId": {
        ".read": true,
        "meta": { ".write": "!data.exists()" },
        "players": {
          "$idx": { ".write": "!data.exists()" }
        }
      }
    }
  }
}
```

---

## ライセンス

MIT
