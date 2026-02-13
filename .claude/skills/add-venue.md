# 新しい会場を追加

## 必要な情報
- 会場名
- 住所
- 座標（latitude, longitude）→ わからなければGoogle Mapsで検索して取得
- 収容人数
- 種別: event（イベント会場）/ hotel / long_spot

## 手順
1. Supabaseのflow_venuesテーブルにINSERT
2. 種別がhotelまたはlong_spotの場合、ナビタブのデータ配列にも追加（座標・名前・詳細情報）
3. 種別がeventの場合、イベントタブの会場リストにも反映されるか確認
4. git commit & push

## 注意
- 座標は小数点以下4桁まで（例: 34.6937, 135.5023）
- 既に同名の会場がないか事前にSELECTで確認
