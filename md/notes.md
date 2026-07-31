# ノート

## これまでやったこと

- `~/mahjong-score/index.html` をGit管理下に置き、GitHubリポジトリ（https://github.com/arutemameteo/mahjong-score）を作成してpush
- Vercelと連携し、本番URL https://mahjong-score-liart.vercel.app を発行（GitHubのmainブランチにpushすると自動デプロイされる設定済み）
- 画像読み取り機能のデバッグ・修正
  - iPhoneで撮影したHEIC画像をAPIに送るとClaude APIが`image/heic`を受け付けず400エラーになる問題を修正。Canvas経由でJPEGに変換してから送信するように変更
  - APIキー貼り付け時に改行・空白が混入し401エラー（invalid x-api-key）になる問題を修正。空白文字を全て除去するサニタイズ処理を追加
  - HEICデコードが`<img>`タグ経由では失敗するケースがあったため、`createImageBitmap()`を優先的に使う方式に変更（失敗時は`<img>`タグ方式にフォールバック）
  - 「この画像形式は読み込めませんでした」エラーを調査 → HEICのネイティブデコードはSafari（Apple独自のImageIO）のみ対応で、Chrome/Edge/Firefox等では`createImageBitmap`・`<img>`タグいずれも構造的に失敗することが判明。Safari限定の仕様として割り切り、非Safariでの失敗時はSafariで開くよう案内するエラーメッセージに変更。実機（iPhone Safari）でHEIC読み取り成功を確認済み
- ヘッダーのタイトル下にバージョン表示（`v{APP_VERSION} build.{BUILD_NUMBER}`）を追加。現在 build.5
- スコア入力欄（東西南北）にmin/max・stepを設定し、入力欄の右に`00`を固定表示する方式に変更（build.3〜5）
  - 入力欄は上位桁のみ（例：25000点 → `250`と入力）、右に固定の`00`ラベルを表示
  - `getSeats()`で読み取り値を×100してから計算ロジックへ渡す。計算ロジック自体は変更なし
  - OCR読み取り結果も÷100して入力欄に反映
  - HTML属性: `min="-1000" max="2000" step="1"`、blur時にJSでクランプ（-1000〜2000の範囲）
  - `SCORE_MIN=-1000, SCORE_MAX=2000`の定数を追加

## 注意事項

- index.htmlを変更するたびに`BUILD_NUMBER`をインクリメントし、ビルド番号をユーザーに伝えること
