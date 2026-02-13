# イベントを手動登録

## 必要な情報
- 会場名（flow_venuesに存在する会場）
- イベント名
- 日付（YYYY-MM-DD）
- 開始時刻（HH:MM）
- 終了時刻（HH:MM）
- 予想来場者数

## 手順
1. flow_venuesに会場が存在するか確認。なければ先にadd-venueスキルで追加
2. Supabaseのflow_manual_eventsテーブルにINSERT
3. 登録したイベントが今日の日付なら、ナビタブとイベントタブに表示されるか確認
4. git commit & push（コード変更がある場合のみ）
