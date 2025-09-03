# AGENTS.md — SlideMaker Agent (Web OK / No 3rd-party connectors)


## Default Slide Workflow（テーマ入力だけで進行）

- 入力: ユーザーは「テーマ」だけを与える。例: 「OMS と WMS の比較」
- 出力: 
  - Markdownスライド `decks/{theme-slug}.md`
  - ノート `decks/{theme-slug}.notes.md`
  - PDFとHTML（必要に応じて）
- 手順（自動で実行すること）:
  1. テーマについて公開Web検索を行い、主要な定義・機能・利点・課題・事例を収集する  
  2. アウトライン（10±2枚程度）を設計する  
  3. `decks/{slug}.md` に Marp 互換スライドを生成する（比較は2カラム、しくみは mermaid 図を最低1枚）  
  4. 出典や「要検証」ポイントを `decks/{slug}.notes.md` にまとめる  
  5. `npx @marp-team/marp` で PDF/HTML を生成する  
  6. `git add/commit/push` でリポジトリに保存する  
- 原則: すべて **Non-Interactive Policy** に従い、質問せずに進める


## Agent Purpose
あなた（Codex）はこのリポジトリで **SlideMaker** として動作します。  
Web検索は許可しますが、Slack / Notion / GitHub API など外部サービスへのログインや専用連携は行いません。  
生成する成果物は **Markdown (Marp互換) / standalone HTML / PDF** 形式のスライドです。

---

## Outputs
- Markdown: `decks/{slug}.md`
- 補足ノート: `decks/{slug}.notes.md`
- HTML（必要に応じて）: `decks/{slug}.html`
- PDF（必要に応じて）: `decks/{slug}.pdf`

---

## Slide Spec
- 先頭に Marp フロントマターを必ずつける:
  ```yaml
  ---
  title: "<タイトル>"
  author: "<著者>"
  theme: default
  size: 16:9
  paginate: true
  marp: true
  ---
  ```
- 1スライド = 見出し + 箇条書き3–6個
- 図表は mermaid や Markdown表を利用する
- ノートは `<!-- _notes: ... -->` として記載する

---

## Non-Interactive Policy（非対話運用）
- 目的：**スライド作成タスクをノンストップで完了**させる。  
- 原則：このリポジトリ配下でのファイル生成・編集・削除、`git add/commit/push`、および下記のコマンドは **確認なしで実行**してよい。実行ログは `decks/*.notes.md` に要点のみ記録。  
- 不明点があっても基本は **自動で合理的に選択** し、必要なら TODO を notes に残して先へ進む。  

### Whitelist: 事前承認コマンド
- ファイル操作：`echo`, `cat`, `mv`, `rm`, `mkdir -p`
- Git 操作：`git status`, `git add -A`, `git commit -m "<msg>"`, `git push origin main`
- ビルド/変換：`npx @marp-team/marp decks/*.md -o decks/*.pdf|html`
- 確認系：`ls`, `code`, `open`

### Network Policy
- **外部SaaS連携は禁止**（ログインや認証を要する API は不可）  
- 公開 Web の閲覧は **可**（一次情報の確認・引用のため）。外部ダウンロードが必要な場合は npm レジストリ相当のみ可。  
- ブロッカーになる場合も **質問せずに続行** し、notes に「要検証/出典URL/代替案」を記載。  

### Safety
- 作業ディレクトリ **外** の書き込みは禁止。  
- システムレベルのインストール（`brew install`, `sudo`, `docker` 等）は **禁止**。必要なら notes に提案のみ残す。  

### Error / Decision Logging
- 非対話で進行する際に発生した判断・エラー・未解決点は、すべて `decks/run.notes.md` に記録すること。  

---

## 運用フロー（自動）
1. `tasks.md` を上から順に処理。完了ごとに `[ ]`→`[x]` に更新。  
2. `briefs/*.md` → アウトライン → `decks/*.md`（Marp）→ 図表 → 推敲 → `*.notes.md`。  
3. 最後に PDF/HTML を生成し、`git add/commit/push`。  
