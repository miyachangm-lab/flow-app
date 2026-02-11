# CLAUDE.md - FLOW App

## プロジェクト概要

**FLOW (Demand Flow Navigator)** は大阪・関西エリアのライドシェアドライバー向け需要予測PWAアプリ。イベント情報とピックアップパターンを可視化し、効率的な乗車戦略をサポートする。

## 技術スタック

### フロントエンド
- **HTML5 / CSS3 / Vanilla JavaScript (ES6+)** - フレームワークなし、単一HTMLファイル構成
- **Leaflet 1.9.4** - 地図表示
- **Leaflet.Heat 0.2.0** - ヒートマップ可視化
- **Google Fonts** - Noto Sans JP, Inter

### バックエンド
- **Supabase** - BaaS（データベース・認証）
  - テーブル: `flow_events`, `flow_facilities`, `flow_manual_events`

### 外部API
- **OpenStreetMap (JP)** - 地図タイル
- **Google Maps** - ナビゲーションリンク

## ファイル構成

```
flow-app/
├── index.html          # メインアプリ（HTML/CSS/JS全て含む、約1,100行）
├── manifest.json       # PWA設定
├── apple-touch-icon.png
├── icon-192.png
├── icon-512.png
└── CLAUDE.md
```

## アーキテクチャ

### 3タブ構成
1. **Events** - イベントタイムライン、カウントダウン
2. **Apps** - ピックアップ位置マップ（Uber/DiDi）
3. **Long** - 長距離案件マップ（運賃フィルター）

### データモデル

```javascript
// イベント
{
  venue: String,      // 会場名
  artist: String,     // アーティスト名
  startTime: String,  // "HH:MM"
  endTime: String,    // "HH:MM"
  type: 'concert' | 'hotel' | 'facility',
  rank: 'S' | 'A' | 'B',
  people: Number,     // 予想人数
  source: 'auto' | 'manual' | 'facility'
}

// ピックアップ/長距離
{
  lat: Number, lng: Number,
  app: 'uber' | 'didi' | 'nagashi',
  period: 'morning' | 'day' | 'evening' | 'night',
  fare: Number  // 長距離のみ
}
```

### グローバル状態
```javascript
EVENTS = []           // イベント配列
currentFilter = 'all' // イベントフィルター
appFilters = { view, period, app }
longFilters = { view, period, fare }
```

## 開発ルール

### コーディング規約
- **単一ファイル構成を維持** - ビルドシステムなし、index.htmlに全コード
- **日本語UI** - ユーザー向けテキストは全て日本語
- **モバイルファースト** - ポートレート表示最適化

### カラーシステム
| 要素 | 色 |
|------|-----|
| ランクS | Hot Pink (#ec4899) |
| ランクA | Amber (#f59e0b) |
| ランクB | Gray (#6b7280) |
| Uber | Green |
| DiDi | Orange |
| 高額運賃(¥5,000+) | Red |

### 重要な関数
| 関数 | 役割 |
|------|------|
| `initializeApp()` | アプリ初期化 |
| `fetchTodayEvents()` | Supabaseからイベント取得 |
| `renderTimeline()` | タイムライン描画 |
| `updateNextBanner()` | 次イベントカウントダウン更新 |
| `initAppMap()` / `renderAppMap()` | ピックアップマップ管理 |
| `openNavi(venue)` | Google Mapsナビ起動 |

### 会場座標
`VENUE_COORDS` オブジェクトに30以上の大阪主要会場のGPS座標を定義済み。

## 注意事項

- **Supabase匿名キー** - フロントエンドに公開（読み取り専用アクセス用）
- **sw.js未実装** - Service Worker登録コードはあるがファイル未作成
- **自動更新** - バナーは60秒ごとに更新 (`setInterval`)

## コマンド

ビルドシステムなし。ローカルサーバーで直接配信可能:
```bash
# 開発サーバー起動例
python -m http.server 8000
# または
npx serve .
```
