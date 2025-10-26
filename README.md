# Tutorial Widget

Webアプリケーションに簡単に組み込めるチュートリアルウィジェットです。初回アクセス時にモーダルダイアログを表示し、複数ページのガイドを提供できます。

## 特徴

- ✨ **初回アクセス時の自動表示** - LocalStorageを使用して初回訪問者のみに表示
- 📄 **複数ページ対応** - JSONファイルで簡単にページ設定を管理
- 🎯 **要素ハイライト機能** - 特定のDOM要素を強調表示し、吹き出し形式で説明を表示
- 🎨 **レスポンシブデザイン** - モバイル・タブレット・デスクトップに対応
- ⚙️ **カスタマイズ可能** - CSSで自由にデザインを変更可能
- 🚀 **軽量・依存関係なし** - 外部ライブラリ不要、純粋なJavaScript
- 🔧 **簡単な統合** - 3ステップで既存のWebアプリに組み込み可能

## デモ

`index.html`をブラウザで開くと、デモページが表示されます。

```bash
# ローカルサーバーで起動する場合（推奨）
npx serve .
# または
python -m http.server 8000
```

ブラウザで `http://localhost:8000` を開いてください。

## インストール

### 1. ファイルのダウンロード

以下のファイルをプロジェクトにコピーします：

```
your-project/
├── tutorial-widget.js
├── tutorial-widget.css
├── tutorial-config.json
└── pages/
    ├── page1.html
    ├── page2.html
    └── page3.html
```

### 2. HTMLに読み込み

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My App</title>

  <!-- Tutorial Widget CSS -->
  <link rel="stylesheet" href="tutorial-widget.css">
</head>
<body>
  <!-- あなたのアプリのコンテンツ -->

  <!-- Tutorial Widget JS -->
  <script src="tutorial-widget.js"></script>
  <script>
    document.addEventListener('DOMContentLoaded', async () => {
      const widget = new TutorialWidget({
        configUrl: 'tutorial-config.json',
        storageKey: 'my-app-tutorial-dismissed' // オプション
      });
      await widget.init();
    });
  </script>
</body>
</html>
```

### 3. 設定ファイルの編集

`tutorial-config.json`で表示するページを設定します：

```json
{
  "pages": [
    {
      "url": "pages/page1.html"
    },
    {
      "url": "pages/page2.html",
      "highlightId": "target-element-id"
    },
    {
      "url": "pages/page3.html"
    }
  ]
}
```

#### ハイライト機能の使用

`highlightId` パラメータを指定すると、そのページでは指定されたDOM要素がハイライトされます：

- ハイライトされた要素は青い枠線で強調表示されます
- それ以外の領域は自動的にグレーアウトされます
- チュートリアル内容は吹き出し形式で表示されます
- 吹き出しの矢印の向きは自動的に最適な位置に調整されます

**例:**
```json
{
  "url": "pages/button-tutorial.html",
  "highlightId": "submit-button"
}
```

## 使用方法

### 基本的な使い方

```javascript
const widget = new TutorialWidget({
  configUrl: 'tutorial-config.json',  // 必須: 設定ファイルのパス
  storageKey: 'my-tutorial-dismissed' // オプション: LocalStorageのキー名
});

await widget.init();
```

### パラメータ

| パラメータ | 型 | 必須 | デフォルト値 | 説明 |
|----------|------|------|------------|------|
| `configUrl` | string | ✓ | - | 設定JSONファイルのURL |
| `storageKey` | string | - | `'tutorial-widget-dismissed'` | LocalStorageに保存するキー名 |

### ページコンテンツの作成

各ページのHTMLファイルには、モーダル内に表示したい内容を記述します：

```html
<div>
  <h2>ページタイトル</h2>
  <p>ここに説明文を記載します。</p>
  <ul>
    <li>リストアイテム1</li>
    <li>リストアイテム2</li>
  </ul>
  <img src="screenshot.png" alt="スクリーンショット">
</div>
```

**注意**: 完全なHTML文書ではなく、HTMLフラグメントとして作成してください。`<html>`、`<head>`、`<body>`タグは不要です。

## API

### メソッド

#### `init()`

ウィジェットを初期化して表示します。

```javascript
await widget.init();
```

#### `close()`

モーダルを閉じます。

```javascript
widget.close();
```

#### `TutorialWidget.reset(storageKey)`

表示状態をリセットします（開発・デバッグ用）。

```javascript
TutorialWidget.reset('my-app-tutorial-dismissed');
```

## カスタマイズ

### スタイルのカスタマイズ

`tutorial-widget.css`を編集することで、デザインをカスタマイズできます。

主なCSSクラス：

| クラス名 | 説明 |
|---------|------|
| `.tutorial-widget-overlay` | オーバーレイ背景（モーダルモード） |
| `.tutorial-widget-modal` | モーダル本体 |
| `.tutorial-widget-tooltip` | ツールチップ形式のモーダル（ハイライトモード） |
| `.tutorial-widget-highlight-box` | ハイライトボックス |
| `.tutorial-widget-close` | 閉じるボタン |
| `.tutorial-widget-content` | コンテンツエリア |
| `.tutorial-widget-footer` | フッターエリア |
| `.tutorial-widget-nav-btn` | ナビゲーションボタン |
| `.tutorial-widget-checkbox` | チェックボックス |

### 配色の変更例

```css
/* プライマリカラーを変更 */
.tutorial-widget-nav-btn {
  background-color: #28a745; /* 緑色に変更 */
}

.tutorial-widget-nav-btn:hover {
  background-color: #218838;
}

/* モーダルの角を丸くする */
.tutorial-widget-modal {
  border-radius: 20px;
}
```

## ファイル構成

```
tutorial-widget/
├── README.md                   # このファイル
├── index.html                  # デモページと利用ガイド
├── tutorial-widget.js          # ウィジェット本体（必須）
├── tutorial-widget.css         # スタイルシート（必須）
├── tutorial-config.json        # 設定ファイル（必須）
└── pages/                      # チュートリアルページ
    ├── page1.html             # 1ページ目
    ├── page2.html             # 2ページ目
    └── page3.html             # 3ページ目
```

## ブラウザ対応

- ✅ Chrome (最新版)
- ✅ Firefox (最新版)
- ✅ Safari (最新版)
- ✅ Edge (最新版)
- ✅ モバイルブラウザ

**必要な機能**:
- LocalStorage
- Fetch API
- ES6 Classes
- Async/Await

## トラブルシューティング

### チュートリアルが表示されない

1. ブラウザの開発者ツール（Console）でエラーを確認
2. `configUrl`のパスが正しいか確認
3. `pages/`内のHTMLファイルが存在するか確認
4. ローカルサーバーで実行しているか確認（`file://`プロトコルではFetch APIが制限される場合があります）

### 表示状態をリセットしたい

```javascript
// JavaScript Consoleで実行
TutorialWidget.reset('my-app-tutorial-dismissed');
location.reload();
```

または

```javascript
// LocalStorageを直接削除
localStorage.removeItem('my-app-tutorial-dismissed');
location.reload();
```

### カスタムイベントを追加したい

`tutorial-widget.js`を編集して、カスタムイベントを追加できます：

```javascript
// 例: ページ変更時のイベント
async showPage(pageIndex) {
  // 既存のコード...

  // カスタムイベントを発火
  window.dispatchEvent(new CustomEvent('tutorial-page-changed', {
    detail: { pageIndex, page: this.pages[pageIndex] }
  }));
}
```

## ライセンス

MIT License

このウィジェットは自由に使用、改変、配布できます。

## 貢献

バグ報告や機能追加のリクエストは、Issueを作成してください。

## 作者

Tutorial Widget Project

---

**Happy Coding!** 🚀
