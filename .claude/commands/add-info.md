---
description: zoobaに関する新しい情報を受け取り、パートナーポータル（zooba_portal.html）の適切なセクションに自動配置・追加するスキル
---

# zooba 情報追加スキル

ユーザーから提供された新しい情報を、zooba_portal.html の最適なセクションに追加します。

## 実行手順

### 1. 情報の分析
ユーザーが提供した情報の内容を分析し、以下のどのセクションに該当するか判定する：

| セクション | 判定基準 |
|-----------|---------|
| FAQ | Q&A形式の情報、よくある質問、製品仕様に関する質疑 |
| ヘルプセンター | 操作手順、設定方法、機能説明、ハウツー記事 |
| 顧客課題一覧 | 顧客が抱える課題、ヒアリング項目、解決方法 |
| アップデート履歴 | 新機能、バグ修正、バージョン情報 |
| zooba進化の流れ | ロードマップ、マイルストーン、沿革 |
| AIヘルプデスク設定 | AIヘルプデスクの設定手順、構成 |
| ITAM セットアップ | IT資産管理の初期設定手順 |
| 導入後運用 | 運用フロー、定期作業、ベストプラクティス |
| デモトレーニング | デモシナリオ、トレーニング資料 |
| RAG設計 | RAG構成、ナレッジベース設計 |
| ヒアリングチェックリスト | 商談ヒアリング項目 |

### 2. 既存データの確認
- zooba_portal.html を読み込み、該当セクションの現在の構造を確認する
- 重複する情報がないかチェックする

### 3. HTMLの追加
- 該当セクションの既存フォーマットに完全に合わせてHTMLを生成する
- **Pythonスクリプトで追加する**（ファイルが大きいためEditツールではなくPython string操作を使う）
- 番号は既存の連番に続ける

### 4. インデックスの更新
- `.claude/zooba_index.json` に新しい情報を追加する

### 5. 確認とプッシュ
- プレビューで追加結果を確認する
- git add → commit → push する

## セクション別の追加フォーマット

### FAQ追加時
```html
<div class="faq-item" data-cat="{カテゴリ}" data-search="{質問}{回答}">
  <div class="faq-q">
    <span class="faq-no">Q{番号}</span>
    <span class="faq-cat-badge">{カテゴリ}</span>
    {質問文}
  </div>
  <div class="faq-a">{回答文}</div>
</div>
```
- 適切なグループセクション（fsec-service/fsec-demo/fsec-it/fsec-emp/fsec-ai/fsec-ops/fsec-other）の末尾に追加
- Q番号は全体の通し番号を振り直す
- サイドバーとトップバーの件数バッジを更新

### ヘルプセンター追加時
```html
<div class="hc-item" data-cat="{カテゴリ}" data-search="{タイトル}{サマリー}">
  <div class="hc-header">
    <span class="hc-no">HC#{番号}</span>
    <span class="hc-cat-badge">{カテゴリ}</span>
    <span class="hc-subcat">{サブカテゴリ}</span>
    <span class="hc-title">{タイトル}</span>
    <a href="{URL}" target="_blank" class="hc-link">→ 記事を開く</a>
  </div>
  <div class="hc-summary">{記事の要約}</div>
  <div class="hc-notes"><span class="notes-label">CS備考</span>{CS向けメモ}</div>
</div>
```
- 該当カテゴリのセクション（hsec-guide/hsec-app/hsec-ai/hsec-device/hsec-emp/hsec-survey/hsec-other）末尾に追加

### 顧客課題追加時
```html
<div class="issue-card2" data-search="{タイトル}{ヒアリング}{解決方法}{Tips}">
  <div class="issue-card2-header" onclick="toggleIssue('{id}')">
    <span class="issue-no2" style="background:{セクション色}">{番号}</span>
    <span class="issue-title2">{課題タイトル}</span>
    <span class="issue-chevron">▼</span>
  </div>
  <div class="issue-card2-body" id="{id}">
    <div class="issue-field">
      <div class="issue-field-label">ヒアリング項目</div>
      <div class="issue-field-content">{ヒアリング質問}</div>
    </div>
    <div class="issue-field">
      <div class="issue-field-label">Zoobaでの解決方法</div>
      <div class="issue-field-content">{解決方法}</div>
    </div>
    <div class="issue-field tips">
      <div class="issue-field-label">営業Tips</div>
      <div class="issue-field-content">{営業Tips}</div>
    </div>
  </div>
</div>
```

### アップデート履歴追加時
- 既存の履歴リストの先頭に追加（最新が上）
- 日付・バージョン・内容を含める

## セクション色マップ

### FAQ グループ色
| グループ | ID | 色 |
|---------|-----|-----|
| サービス概要・料金・競合 | fsec-service | #2980B9 |
| 導入・デモ・トライアル | fsec-demo | #27AE60 |
| IT資産管理 | fsec-it | #C0392B |
| 従業員・棚卸し管理 | fsec-emp | #1F7A8C |
| AIヘルプデスク・ナレッジ | fsec-ai | #8E44AD |
| 運用・CS・セキュリティ | fsec-ops | #D35400 |
| その他 | fsec-other | #7F8C8D |

### 顧客課題セクション色
| セクション | 色 |
|-----------|-----|
| SaaS・アプリケーション管理 | #8E44AD |
| デバイス管理 | #C0392B |
| 従業員・権限管理 | #1F7A8C |
| 棚卸し・コンプライアンス | #D35400 |
| AIヘルプデスク・情シス運用 | #1F4E79 |

## 重要なルール

- zooba_portal.html の編集は必ず **Pythonスクリプト** で行う（ファイルサイズが大きいため）
- 追加後は件数バッジ（サイドバー・トップバー両方）を更新する
- 既存のフォーマット・スタイルを完全に踏襲する
- 追加後にプレビュー確認→git push まで一貫して行う

$ARGUMENTS
