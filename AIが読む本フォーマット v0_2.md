AIが読む本フォーマット v0.2

1. 概要

AIが本の内容を構造的に扱えるようにするためのフォーマット。

目的は意味を一つに固定することではなく、以下を分離して保存することにある。

本文で記述された内容

それに対する解釈

未確定の領域

対象ジャンル：

小説

エッセイ

論文

思想書

回想録

2. 設計思想

このフォーマットの基本原則：

fact と interpretation を分離する

uncertainty を内容として保存する

解釈には主体を付ける

因果は断定せず候補関係も保存する

構造は意味を固定するためではなく、意味の可能性と限界を保存するために使う

3. コア構造

v0.2では以下の6要素をコアとする。

metadata（narrator を含む）
structure
content_units
interpretations
uncertainties
relations

4. 各要素仕様

metadata

書誌情報。v0.2から narrator フィールドを追加。

metadata:
  title:
  author:
  genre:
  language:
  narrator:                  # optional
    specified: true / false  # 語り部を設定するか否か。false も意図的な選択として記録する
    identity: protagonist / third_party / unknown
    note:                    # 語り部を固定する理由、または意図的に未設定にする理由

narrator について

interpretations の agent: narrator は「語り部の視点からの解釈」を意味する。
v0.1ではその語り部が誰であるかが未定義だったため、v0.2で metadata に明示する。
specified: false は「語り部を意図的に曖昧にする」という選択を記録するためのフィールド。
省略した場合は specified: false と同等に扱う。

identity 語彙（v0.2）

protagonist    語り部が物語の当事者（加害者・被害者・主人公など）
third_party    語り部が物語の外部にいる観察者
unknown        語り部が誰かを確定しない、またはできない

structure

本文の配置構造。
意味構造ではなく章や節などの区切りを示す。

structure:
  units:
    - id:
      type:
      title:

例：

chapter

section

part

scene

content_units

本文に記述されている内容の最小単位。

重要ルール：

description must describe what is stated, observed, or explicitly reported, not inferred meaning

つまり以下のみを記録する。

出来事

観察

発言

状態

解釈はここに入れない。

content_units:
  - id:
    type:
    location:
    description:

interpretations

主体付きの解釈レコード。

interpretations:
  - id:
    agent:
    target:
    claim:
    confidence:

agent（v0.2）
narrator
reader
critic

confidence
low
medium
high

uncertainties

不確定性を保存する層。

uncertainties:
  - id:
    target:
    type:
    reason:

推奨タイプ：

causal_unknown
mechanism_unknown
evidence_missing
witness_unavailable
scope_unknown
interpretation_conflict

relations

content_units間の関係。

relations:
  - id:
    source:
    target:
    type:

v0.2 relation type：

temporal_sequence
explicit_cause
possible_cause
retrospective_link
contrast
thematic_echo

5. 推奨語彙

content_units.type の推奨セット：

state
event
observation
statement
outcome
condition
trigger
reflection

これは必須語彙ではなく recommended vocabulary。

6. 設計原則

fact と interpretation を分離する

uncertainty を内容として保存する

解釈主体を記録する

語り部（narrator）を明示する。曖昧にする場合もその選択を記録する

relations は確定因果だけでなく候補関係も扱う

構造は意味を固定するためではなく 意味の可能性と限界を保持するために使う

7. v0.3検討事項

将来の拡張候補。

content_units.type語彙体系

agent粒度の細分化（narrator の identity との対応）

narrator.identity に omniscient を追加

relations.type拡張

confidence数値化

source_text フィールドの追加（optional）
構造から原文を再生成する実験により、構造は後味を再現できるが原文の質感（具体的な数値・語彙・間の取り方）は再現できないことが確認された。原文を保持することで検証精度が上がる。ただし著作権のある既存テキストには適用できないため必須ではなく optional とする。

Appendix

実例は [`お不動さん.md`](お不動さん.md) を参照。
