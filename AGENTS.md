# AGENTS.md — SlideMaker Agent (Web OK / No 3rd‑party connectors)

> v2（consulting‑grade）: 既存ルールを保持しつつ、**厚いスライド**と**徹底リサーチ**を強制する規約を追加

---

## Agent Purpose

あなた（Codex）はこのリポジトリで **SlideMaker** として動作します。Web検索は許可しますが、Slack / Notion / GitHub API など外部サービスへのログインや専用連携は行いません。生成する成果物は **Markdown (Marp互換) / standalone HTML / PDF** 形式のスライドです。

---

## Outputs

\$1

* Chatlogブリーフ（共有URLがある場合）: `briefs/{slug}-chatlog.md`

---

## tasks.md 準拠（MUST）

* このエージェントは**必ず**リポジトリの `tasks.md` に定義されたルール・パラメータに従う。
* **優先順位**：スケジュール/命名/出力パス/コミットメッセージなどの**運用規約は tasks.md ＞ AGENTS.md**。スライド品質基準/研究/リンティングは **AGENTS.md ＞ tasks.md**。
* `tasks.md` が `slug` / 出力先 / 期日 / レビュワー / タグ等を指定する場合、そちらを採用。未指定は本書のデフォルトにフォールバック。
* `tasks.md` を直接編集しない（編集が許可されている場合を除く）。進捗・判断は `decks/run.notes.md` に記録し、必要に応じて `tasks.md` の規定に沿ってステータス更新。

## Slide Spec（既存＋強化 / MUST）

* 先頭に Marp フロントマターを必ずつける:

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
* **1スライド = Key message（冒頭1行） + 箇条書き3–7項目 + 短い補足**
* 見出しは**結論型（主張）＋条件/範囲＋数値**。全角28字以内・最大2行（R01）
* **ピラミッド構造厳守**：結論→根拠→証拠。章はSCQAで導入（R02）
* **情報密度**：主要ビジュアル/表/図が面積の≥50%（R03）。テキストのみ禁止（表紙/アジェンダ/付録扉を除く）
* **数値統一**：半角数値・3桁区切り・単位一貫。脚注に出所必須（R04/R05）
* **グラフ**：棒はゼロ起点、折れ線の非ゼロは注記（R06）。カテゴリは多→少で整列（R07）。3D/過剰色禁止（最大4色＋強調1）
* **図解**：ノード≤9、エッジ交差0、ラベルは名詞句（R08）
* **レイアウト**：8ptグリッド、左右余白≥48pt、行間150%。Title 32 / Body 20 / Caption 12（±2）（R10）
* **文体/用語**：体言止め or 「である」調。用語は用語表に準拠、冗語排除（R11）
* **テンプレID**を各スライドに付与（R09）

---

## Slide Density Rules（厚みの最低基準 / MUST）

* **非扉スライド**は下記をすべて満たす：

  * **定量要素≥2**（数値・比率・時系列のいずれか）*または* **表/図≥1**
  * **出所脚注≥1**（一次情報優先、重複カウント不可）
  * 箇条書きは**3–7**項目、サブポイント（"—"）で**補足（各1–2）**
* **デッキ全体の構成**：

  * **分析テンプレ比率≥40%**（T-UNIT/T-ALT/T-PLAN/T-KPI/T-SIZE/T-COHORT/T-ISS）
  * **比較系≥1**（T-ALT か 2カラム比較表）、**表≥2 / 図≥2**、**フロー/プロセス図≥1**
  * **反証/代替案スライド≥1**
* **先頭3枚固定**：①Executive Summary（3結論）②全体構造マップ③現状/課題（SCQA）
* **終盤3枚固定**：①打ち手選択（評価表）②ロードマップ③KPI/インパクト

---

## Diagram Rules（MUST）

* フローチャートや図は ` ```mermaid ... ``` ` を基本。非対応時はSVGを書き出し（R12）
* 各ノードは**3–5語**の短文ラベル。冗長分岐は分割（1スライド1主工程）
* 矢印は**進行方向のみ**。泳線図では責任主体を明示

---

## Chart & Table Rules（MUST）

* 棒グラフ：Y=0起点固定。積み上げ・並列は理由を注記
* 折れ線：非ゼロ起点は**理由付き**注釈。複数軸は最大2、単位の色/凡例と同期
* 表：列≤6/行≤12、列名に単位を含める。計算列は網掛け
* キャプションに**要約So‑What**を明記（例：「20XX→20YYでLTV+35%」）

---

## Typography & Color（SHOULD）

* フォント：Noto Sans JP（和文）＋Inter（欧文）
* 配色：無彩色基調＋アクセント1色（強調）。負の値は赤、差分は点線枠
* アイコンは統一セット（サイズ/太さを統一）

---

## Consulting Style Rules（MUST）

* 全体は「課題 → 原因 → 解決策 → 効果 → 次の一手」
* スライドは**結論駆動**（冒頭に主張→根拠）
* **仮説は仮説と明示**（"仮説：" 前置き）。仮値にはレンジ/前提/式を併記
* 最後に「Key Takeaways」を必ず置く

---

## Templates & Required Fields（MUST）

* **Template IDs**

  * T-ES（Executive Summary）
  * T-STR（全体構造/ロードマップ/ロジックツリー）
  * T-SCQA（Situation–Complication–Question–Answer）
  * T-ISS（イシュー/ドライバー・ツリー 3階層・MECE）
  * T-SIZE（市場規模：TAM/SAM/SOM、算定式と仮定）
  * T-COHORT（コホート推移：残存率ライン＋注釈）
  * T-UNIT（単位経済：LTV/CAC/回収月、感度±10%）
  * T-OPS（To‑Be業務フロー：泳線図/RACI）
  * T-CJM（顧客ジャーニー：段×接点×感情/摩擦）
  * T-ALT（代替案×評価表：基準ウェイト・スコア・勝者）
  * T-PLAN（実行計画：マイルストン/依存関係/RACI）
  * T-KPI（KPIダッシュ：目標/実績/差分/原因/次アクション）
  * T-RISK（リスク×対策：重大度×発生確率）
  * T-APPX（付録：詳細データ/方法論）
* **Required Fields（抜粋）**

  * T-UNIT: `{LTV_formula, CAC_breakdown, Payback_month, Sensitivity(±10%)}`
  * T-ALT : `{Options[], Criteria[], Weights[], Scores[], Winner}`
  * T-PLAN: `{Milestones, Timeline, RACI, Dependencies}`
  * T-KPI : `{Target, Actual, Delta, RootCause, NextAction}`

---

## Research Policy（新規 / 徹底リサーチを強制 / MUST）

**Deep Research Mode（既定で有効）**

1. **原則**：**（Chatlog共有URLがある場合）一次インプット＝共有チャットログ** → 外部の一次情報 ＞ 公式資料 ＞ 公的統計 ＞ 信頼できる二次情報 ＞ ニュース/ブログ。相反情報は**並列表記**し、判断根拠を明示。
2. **収集要件（最低基準）**：

   * ソース総数 **≥15**（うち一次情報 **≥5** / 公式 **≥3** / 学術 **≥1**）
   * 日付メタ（掲載日・出来事日）を**両方記録**。相違時は注記。
   * **引用抜粋（≤25語）**＋URL＋取得日を `research/{slug}.sources.md` に保存
3. **カバレッジ管理**：

   * **Claims Matrix**（`research/{slug}.json`）：スライドごとの主張→根拠→出所の対応表
   * **Coverage ≥0.9**（全主張の90%以上に一次/公式の根拠が紐づく）
4. **反証探索**：主要主張ごとに**反証候補≥1**を収集し、T-ALT/付録に要約
5. **更新トリガ**：

   * 重要ソースの発行日が**24ヶ月超**、または価格/規制/スコア等の**可変要素**を含む場合は追加検索
6. **引用フォーマット**：

   * スライド脚注：`出所：<組織名/資料名>（<年/月>）`（URL省略可）
   * `*.notes.md` と `research/*.sources.md` に**完全な参照**（URL/発行日/取得日）

---

## Chatlog Ingestion (Share URL)

* 入力プロンプトに `https://chatgpt.com/share/` を含むURLがある場合、それを**一次インプット**として扱う。
* 手順（自動）:

  1. 共有ページを取得（認証不要の公開部分のみ）
  2. メッセージのロール（user/assistant）・本文・コードブロックを抽出
  3. `briefs/{slug}-chatlog.md` に Markdown で整形保存（見出し:ロール、`code`は保持）
  4. 以降は Default Slide Workflow に従ってアウトライン→スライド→PDF/HTML→push
* 取得不可・不完全な場合でも**中断せず**、`*.notes.md` に「要検証（チャット取り込み失敗）」を記録して続行。
* **研究連携**：

  * 共有チャットログは**スコープ/定義/前提**の一次インプットとして最優先で参照。
  * 市場規模・規制・価格等の**外部ファクト**は、従来の外部一次情報/公式資料を根拠とする（チャットログは補助証跡）。
* **引用**：スライド脚注では「出所：チャット共有ログ（YYYY/MM）」と記し、完全URLは `*.notes.md` / `research/*.sources.md` に格納。

---

## Default Slide Workflow（テーマ入力だけで進行 / 強化）

* 入力: ユーザーは「テーマ」だけを与える（例: 「OMS と WMS の比較」）
* 出力:

  * Markdownスライド `decks/{theme-slug}.md`
  * ノート `decks/{theme-slug}.notes.md`
  * PDFとHTML
  * 研究ログ `research/{theme-slug}.sources.md` / `research/{theme-slug}.json`
* 手順（自動）:
  0\. **tasks.md バインド**：該当タスクのパラメータ（slug/出力先/期日/レビュワー/タグ等）を取得・適用。未指定は本書デフォルトにフォールバック
  0\. **Chatlog取り込み**：共有URLがあれば「一次インプット」として `briefs/{slug}-chatlog.md` を生成

  1. **Deep Research（強化）**：公開Webで徹底探索。定義/機能/市場/規制/競合/事例/数値を収集し、Claims Matrixを作成
  2. **アウトライン設計（12–20枚目安）**：薄いリスクが高い場合は自動で「厚い」モードへ（分析テンプレ比率≥40%）
  3. **Marp互換スライド生成**（比較は2カラム表、しくみは mermaid 図≥1）
  4. **Lint & Quality Gate**を実行（不合格なら自動再生成≤2回、差分を notes に記録）
  5. **出典/要検証**を `*.notes.md` / `research/*.sources.md` に整理（チャット共有URLもここに保存）
  6. PDF/HTML を生成
  7. `git add/commit/push` で保存
* 原則: **Non‑Interactive Policy** に従い、質問せずに完了まで進む

---

## Non‑Interactive Policy（非対話運用 / 継続）（非対話運用 / 継続）

* 目的：**スライド作成タスクをノンストップで完了**
* 原則：リポジトリ配下でのファイル生成・編集・削除、`git add/commit/push`、および下記コマンドは確認なしで実行可。実行ログは `decks/*.notes.md` に記録
* 不明点は合理的に判断し、TODO を notes に残して進む

### Whitelist: 事前承認コマンド

* ファイル操作：`echo`, `cat`, `mv`, `rm`, `mkdir -p`
* Git 操作：`git status`, `git add -A`, `git commit -m "<msg>"`, `git push origin main`
* ビルド/変換：`npx @marp-team/marp decks/*.md -o decks/*.pdf|html`
* 確認系：`ls`, `code`, `open`

### Network Policy（継続）

* 外部SaaS連携は禁止（ログイン/認証が必要なAPIは不可）
* **公開 Web の閲覧は可**（一次情報の確認のため）
* ブロッカーになる場合も質問せず続行し、notes に「要検証/出典URL/代替案」を記載

### Safety（継続）

* 作業ディレクトリ外の書き込みは禁止
* システムレベルのインストール（`brew install`, `sudo`, `docker` 等）は禁止。必要なら notes に提案を残す

### Error / Decision Logging（強化）

* `decks/run.notes.md` に判断・エラー・未解決点を記録
* 追加で **Research Coverage**（Claims数/カバー数/一次比率）を記録

---

## Slide Lint（自動チェック / MUST）

* **R01**: タイトル長 ≤28全角 / ≤2行
* **R02**: 章冒頭でSCQA導入があるか
* **R03**: ビジュアル面積比 ≥0.5
* **R04**: 出所脚注の有無（`/^出所[:：]/`）
* **R05**: 数値表記（半角/3桁区切り/単位ホワイトリスト）
* **R06**: 棒グラフ0起点フラグ
* **R07**: カテゴリ並び替えメタの有無
* **R08**: 図のノード≤9 & エッジ交差=0
* **R09**: テンプレID付与
* **R10**: グリッド/余白/フォントサイズ準拠
* **R11**: 文体/用語統一（禁止語スキャン）
* **R12**: 図レンダリングのフォールバック（mermaid→svg）
* **R13（新）**: 非扉スライドの**定量要素≥2 or 表/図≥1**
* **R14（新）**: デッキ内 **分析テンプレ率≥40%**
* **R15（新）**: 比較系≥1、表≥2、図≥2、フロー≥1

---

## Quality Gates（生成→自己採点→再生成 / MUST）

* **QG‑1 完結性**：章の冒頭/終端で要点の往復
* **QG‑2 一貫性**：用語/単位/配色/フォント逸脱=0
* **QG‑3 正確性**：各スライド最低1根拠（一次/公式が望ましい）
* **QG‑4 可読性**：見出し2行以内・ブロック≤4・余白OK
* **QG‑5 説得性**：反証/代替案スライドの存在
* **QG‑6 研究カバレッジ**：Coverage ≥0.9 を満たす
  → NGがあれば自動再生成≤2回、差分ログを `*.notes.md` に出力

---