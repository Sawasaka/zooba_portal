# 手順書コンテンツ追加・更新プラン

## Context

ユーザーから4つの手順書（Markdown + PDF）が提供された。これらの内容をポータル（zooba_portal.html）に反映する：
1. **Google Chat** AIヘルプデスク手順書 → `ai-tab-gchat` タブ更新 + PDF追加
2. **Teams** AIヘルプデスク手順書 → `ai-tab-teams` タブ更新（既存あり）
3. **Chatwork** AIヘルプデスク手順書 → `ai-tab-chatwork` タブ更新（既存あり）
4. **棚卸しアンケート** 手順書 → 新規セクション `section-survey` 追加

## 方針

- Markdownの内容と**完全一致**させる
- 既存のHTMLパターン（Teamsタブのスタイル等）を踏襲
- 各手順書にPDFダウンロードボタンを設置
- 棚卸しはサイドバー「セットアップ・運用」カテゴリに追加

## 対象ファイル

- `zooba_portal.html` — メイン（CSS/HTML/JS）
- PDFファイル（既存: chatwork/teams/survey/itam、**新規: Google Chat用PDF**）

## 実装手順

### Step 1: Google Chat用PDFファイルを配置
- 提供されたPDFの名前を確認し、リポジトリに配置
- ファイル名: `gchat_gdrive_setup_guide.pdf`

### Step 2: ai-tab-gchat（Google Chat）タブ更新
- 行1937〜付近の既存内容を **全面書き換え**
- Markdownの内容をHTMLに変換（Teams/Chatworkと同じパターン）
- 色テーマ: `#0f9d58`→`#0b7a44` グラデーション
- 構造: ヘッダーバナー + PDFダウンロード + チェックリスト + STEP 1-4 + CS参照リンク集

### Step 3: ai-tab-teams（Teams）タブ更新
- 行1841〜付近の既存内容を **全面書き換え**
- Markdownの内容と完全一致させる
- 色テーマ: `#5b5fc7`→`#464EB8`（既存維持）
- PDF: `teams_sharepoint_setup_guide.pdf`（既存）

### Step 4: ai-tab-chatwork（Chatwork）タブ更新
- 行1622〜付近の既存内容を **全面書き換え**
- Markdownの内容と完全一致させる
- 色テーマ: `#1e88e5`→`#0d47a1`（既存維持）
- PDF: `chatwork_setup_guide.pdf`（既存）
- 注意: 既存のチェックボックスJS（saveCWCheck, cw-progress-bar）を維持するか要検討

### Step 5: section-survey（棚卸しアンケート）新規追加
- サイドバーに項目追加（「セットアップ・運用」カテゴリ内、`導入後運用` の後）
- `sectionMeta` に `survey` エントリ追加
- section-survey のHTML作成（ITAMセクションと同様のパターン）
- 色テーマ: `#e65100`→`#bf360c`（オレンジ系）
- PDF: `survey_setup_guide.pdf`（既存）

### Step 6: sectionMeta 更新
```javascript
survey: { title: '棚卸しアンケート セットアップ', badge: '' },
```

## HTMLパターン（共通テンプレート）

各タブ/セクションは以下の構造で統一：
```
ヘッダーバナー（gradient背景 + タイトル + PDFダウンロードボタン）
├── 事前確認チェックリスト（#fff3e0背景）
│   └── 注意事項ボックス（#fff8e1 + border-left）
├── STEP 1-N（.setup-steps > .setup-step）
│   ├── .step-number（色付き）
│   └── .step-body > .step-title + .step-content + .step-point
└── CS参照リンク集（テーブル）
```

## 検証
- 全タブの表示確認（共通/Chatwork/Slack/Teams/Google Chat/前提ポリシー）
- 棚卸しアンケートセクションの表示確認
- PDFダウンロードボタンの動作確認
- サイドバーからの切り替え確認
