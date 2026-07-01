AIが読む本フォーマット v0.3

---

1. 概要

AIが本の内容を構造的に扱えるようにするためのフォーマット。

目的は意味を一つに固定することではなく、以下を分離して保存することにある。

本文で記述された内容
それに対する解釈
未確定の領域
構造化判断の検証記録（v0.3新規追加）

対象ジャンル：

小説
エッセイ
論文
思想書
回想録
対談・対話記録

---

2. v0.2からの変更点

v0.3の核心的な追加は `structuring_audit` フィールドである。

背景：バタイユ「エロティシズム」の構造化実験において、構造化エージェントが
`content_rule`（「述べられていることを記述する」）の違反と、認識論的問題
（「この主張は検証不能である」）を混同していた。この混同はフォーマット内に
記録されず、結果として構造化データの信頼性が検証不能なままになっていた。

`structuring_audit` はこの問題に対処するための検証層である。

変更一覧：

追加：`structuring_audit`（構造化判断の後検証層）
追加：`content_units` の `source_text` フィールド（optional）
追加：`narrator.identity` に `omniscient`
追加：`relations.type` に `assertion_without_evidence`、`breakdown_echo`、`internal_contradiction`
拡張：`content_units.type` 推奨語彙
更新：`uncertainties.type` 推奨語彙

---

3. 設計思想

このフォーマットの基本原則（v0.2から継続）：

fact と interpretation を分離する
uncertainty を内容として保存する
解釈には主体を付ける
因果は断定せず候補関係も保存する
構造は意味を固定するためではなく、意味の可能性と限界を保存するために使う

v0.3で追加する原則：

構造化判断そのものも検証可能な記録として残す
構造化エージェントの判断誤りは `structuring_audit` に記録する
「記述不能」と「検証不能」は区別する

  記述不能（content_rule違反）：
    inference なしに description を書けない場合
  検証不能（epistemological problem）：
    主張の真偽を確認できない場合
    ただし主張の内容は記述可能

---

4. コア構造

v0.3では以下の7要素をコアとする。

metadata（narrator を含む）
structure
content_units
interpretations
uncertainties
relations
structuring_audit（新規）

---

5. 各要素仕様

metadata

書誌情報。narrator フィールドを含む。

metadata:
  title:
  author:
  genre:
  language:
  narrator:
    specified: true / false
    identity: protagonist / third_party / unknown / omniscient  # omniscient を v0.3 で追加
    note:

narrator.identity 語彙（v0.3）

protagonist    語り部が物語の当事者
third_party    語り部が物語の外部にいる観察者
unknown        語り部が誰かを確定しない、またはできない
omniscient     語り部が登場人物の内面・外部状況を全て知る視点（小説等で使用）


structure

本文の配置構造。意味構造ではなく章・節・場面などの区切りを示す。

structure:
  units:
    - id:
      type:       # chapter / section / part / scene / phase
      title:


content_units

本文に記述されている内容の最小単位。

重要ルール：
  description must describe what is stated, observed, or explicitly reported,
  not inferred meaning

content_units:
  - id:
    type:
    location:
    description:
    source_text:  # optional。原文の該当箇所を保持する。著作権のある既存テキストには適用不可のため任意。

source_text について（v0.3追加）
構造から原文を再生成する実験により、構造は後味を再現できるが
原文の質感（具体的な語彙・間の取り方）は再現できないことが確認されている。
source_text を保持することで structuring_audit の検証精度が上がる。

content_units.type 推奨語彙（v0.3拡張）

state          状態の記述
event          出来事
observation    観察
statement      発言・陳述
outcome        結果
condition      条件
trigger        起点となる出来事・発言
reflection     内省・振り返り
definition     定義の提示（v0.3追加）
claim          根拠付きまたは根拠なしの主張（v0.3追加）
assertion      論証なしの断言（v0.3追加）


interpretations

主体付きの解釈レコード。

interpretations:
  - id:
    agent:        # narrator / reader / critic
    target:
    claim:
    confidence:   # low / medium / high


uncertainties

不確定性を保存する層。

uncertainties:
  - id:
    target:
    type:
    reason:

uncertainties.type 推奨語彙

causal_unknown
mechanism_unknown
evidence_missing
witness_unavailable
scope_unknown
interpretation_conflict
rule_application_ambiguous   # v0.3追加：content_ruleの適用が曖昧な箇所


relations

content_units 間の関係。

relations:
  - id:
    source:
    target:
    type:
    note:

relations.type（v0.3拡張）

temporal_sequence
explicit_cause
possible_cause
retrospective_link
contrast
thematic_echo
assertion_without_evidence   # v0.3追加：根拠なき断言の連鎖
breakdown_echo               # v0.3追加：あるcontent_unitの破綻が後続unitに波及する関係
internal_contradiction       # v0.3追加：同一著者・話者が同一テキスト内で矛盾する主張をしている関係。執筆・編集時の論旨整合性チェックにも使用可能


structuring_audit（v0.3新規追加）

構造化判断を後から検証するための層。
構造化エージェントの判断誤りをここに記録する。
auditor は構造化エージェントと異なる主体（別のAI・人間）が担うことが望ましい。

structuring_audit:
  - id:
    target:           # 検証対象の content_unit または relation の id
    auditor:          # 検証した主体（human / second_ai / etc.）
    issue_type:       # 下記語彙参照
    description:      # 何が問題だったかの記述
    correction:       # 正しい適用はどうあるべきだったか
    revised_field:    # optional。修正が必要なフィールドと修正内容

structuring_audit.issue_type 語彙

rule_misapplication
  content_rule の誤適用。
  「記述不能」と「検証不能」の混同が典型例。
  評価的概念を含む主張でも「著者がそう述べている」という事実は記述可能。

scope_conflation
  異なる問題種別の混同。
  認識論的問題・論理的問題・フォーマット上の問題を区別せずに
  breakdown_point として処理した場合など。

agent_bias
  構造化エージェントがプロンプトや文脈に引きずられ、
  特定の結論（破綻・高評価など）に向けて構造化した可能性がある場合。
  目的設定が "どこで破綻するかを記録する" のように誘導的だった場合に発生しやすい。

overconfident_breakdown
  breakdown_point: true の宣言根拠が不十分。
  判断は可能だが確信度が不明な場合。

confirmed
  構造化判断が検証の結果正しいと確認された場合。

---

6. 設計原則（v0.3）

fact と interpretation を分離する
uncertainty を内容として保存する
解釈主体を記録する
語り部（narrator）を明示する。曖昧にする場合もその選択を記録する
relations は確定因果だけでなく候補関係も扱う
構造は意味を固定するためではなく 意味の可能性と限界を保持するために使う
構造化判断そのものを検証可能な記録として残す（v0.3追加）
「記述不能」と「検証不能」を区別する（v0.3追加）
同一テキスト内の矛盾は internal_contradiction として記録する（v0.3追加）

---

7. v0.4 検討事項

confidence の数値化（0.0–1.0）
  ただし偽精度になるリスクがあるため、low/medium/high の定義精緻化を先に行う。

agent 粒度の細分化
  narrator.identity との対応関係の整理。

structuring_audit の自動化
  structuring_audit を別エージェントが自動実行するプロセスの設計。
  フォーマットの問題ではなく運用設計の問題として切り出す。

複数エージェントによる構造化の差分記録
  同一テキストを異なるAIが構造化した場合の差分をどう保存するか。

---

Appendix

実例：
  対談記録への適用 — 対談構造化_AIとミュージシャン論.md
  哲学書への適用 — bataille_erotisme_structuration.md（v0.2時点の実験。structuring_auditによる再検証が必要）
