# ノート

## これまでやったこと

- `~/mahjong-score/index.html` をGit管理下に置き、GitHubリポジトリ（https://github.com/arutemameteo/mahjong-score）を作成してpush
- Vercelと連携し、本番URL https://mahjong-score-liart.vercel.app を発行（GitHubのmainブランチにpushすると自動デプロイされる設定済み）
- 画像読み取り機能のデバッグ・修正
  - iPhoneで撮影したHEIC画像をAPIに送るとClaude APIが`image/heic`を受け付けず400エラーになる問題を修正。Canvas経由でJPEGに変換してから送信するように変更
  - APIキー貼り付け時に改行・空白が混入し401エラー（invalid x-api-key）になる問題を修正。空白文字を全て除去するサニタイズ処理を追加
  - HEICデコードが`<img>`タグ経由では失敗するケースがあったため、`createImageBitmap()`を優先的に使う方式に変更（失敗時は`<img>`タグ方式にフォールバック）

## 注意事項

- Vercelビルド時にビルド番号を知らせること
