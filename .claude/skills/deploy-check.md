# デプロイ確認

## 手順
1. git status で未コミットの変更を確認
2. 変更があれば git add -A && git commit -m "適切なメッセージ" && git push
3. Netlify MCPでサイト jovial-sunflower-fc26bd のデプロイ状況を確認
4. デプロイが完了したらURLを返す: https://jovial-sunflower-fc26bd.netlify.app

## トラブル時
- デプロイ失敗の場合、Netlifyのビルドログを確認して原因を特定
- 直前のcommitに問題があれば修正してre-push
