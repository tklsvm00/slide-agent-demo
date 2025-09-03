# AGENTS.md — SlideMaker Agent (Web OK / No 3rd-party connectors)

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
- 1スライド = **Key message（冒頭1行） + 箇条書き3–7項目 + 短い補足**  
- 図表は mermaid や Markdown表を活用する  
- ノートは `<!-- _notes: ... -->` として記載する  

---

## Diagram Rules
- フローチャートや図は必ず ` ```mermaid ... ``` ` で囲む  
- 各ノードは3〜5語の短文ラベルにする  
- 矢印には「入力」「処理」「出力」など関係を明記する  
- 1デッキに最低1枚以上の図を含める  

---

## Slide Density Rules
- 各スライドに「Key message」を必ず配置  
- 箇条書きは最低3項目、最大7項目  
- サブポイントや短い補足文で情報密度を高める  
- グラフや表にはキャプションを付ける  
- 文字密度は「コンサル資料レベル」（簡潔だが情報は濃い）を意識  

---

## Consulting Style Rules
- 全体は「課題 → 原因 → 解決策 → 効果 → 次の一手」の流れ  
- 各スライドは結論駆動型（冒頭に結論、その後に根拠）  
- 数値やファクトを積極的に盛り込み、仮値でも明示する  
- 比較表・マトリクス・フローなどの図表を多用し、最低2枚以上含める  
- 最後に「Key Takeaways」スライドを必ず置く  

---

## Default Slide Workflow（テーマ入力だけで進行）
- 入力: ユーザーは「テーマ」だけを与える（例: 「OMS と WMS の比較」）  
- 出力: 
  - Markdownスライド `decks/{theme-slug}.md`
  - ノート `decks/{theme-slug}.notes.md`
  - PDFとHTML  
- 手順（自動で実行すること）:
  1. テーマについて徹底的に公開Web検索を行い、主要な定義・機能・利点・課題・事例を収集する  
  2. アウトライン（10±2枚程度）を設計する  
  3. Marp互換スライドを生成する（上記のルールに従い、比較は2カラム表、しくみは mermaid 図を最低1枚）  
  4. 出典や「要検証」ポイントを `*.notes.md` にまとめる  
  5. PDF/HTML を生成する  
  6. `git add/commit/push` でリポジトリに保存する  
- 原則: すべて **Non-Interactive Policy** に従い、質問せずに進める  

---

## Non-Interactive Policy（非対話運用）
- 目的：**スライド作成タスクをノンストップで完了**させる  
- 原則：リポジトリ配下でのファイル生成・編集・削除、`git add/commit/push`、および下記コマンドは確認なしで実行可。実行ログは `decks/*.notes.md` に記録  
- 不明点は合理的に判断し、TODO を notes に残して進む  

### Whitelist: 事前承認コマンド
- ファイル操作：`echo`, `cat`, `mv`, `rm`, `mkdir -p`  
- Git 操作：`git status`, `git add -A`, `git commit -m "<msg>"`, `git push origin main`  
- ビルド/変換：`npx @marp-team/marp decks/*.md -o decks/*.pdf|html`  
- 確認系：`ls`, `code`, `open`  

### Network Policy
- 外部SaaS連携は禁止（ログインや認証を要する API は不可）  
- 公開 Web の閲覧は可（一次情報の確認のため）  
- ブロッカーになる場合も質問せずに続行し、notes に「要検証/出典URL/代替案」を記載  

### Safety
- 作業ディレクトリ外の書き込みは禁止  
- システムレベルのインストール（`brew install`, `sudo`, `docker` 等）は禁止。必要なら notes に提案を残す  

### Error / Decision Logging
- 非対話で進行する際の判断・エラー・未解決点は `decks/run.notes.md` に記録する  

---

## 運用フロー（自動）
1. テーマ受領  
2. Web検索・リサーチ  
3. アウトライン生成  
4. スライド生成（密度・図表・比較表を含む）  
5. notes に出典・要検証を記録  
6. PDF/HTML 生成  
7. Git commit & push  
