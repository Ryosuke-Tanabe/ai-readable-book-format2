# AIが読む本フォーマット v0.3

AIが文章の主旨を追跡できるようにするための構造化フォーマット。

テキストを `content_units`・`relations`・`interpretations`・`uncertainties` に分解してYAMLとして記述する。v0.3から `structuring_audit`（構造化判断の検証層）を追加。

→ 背景と発見：[AIが読む本を作ったら、対話の構造的矛盾が見えた](https://note.com/moral_spirea2538/n/n71465bec43f1)（note）
→ 哲学書への適用実験（v0.2）：[バタイユを構造化したら、破綻した。それが批評になった。](https://note.com/moral_spirea2538/n/nd7f5b6afe1da)（note）※この記事の結論は誤りだった
→ 哲学書への適用実験（v0.3）：[バタイユを構造化したら、破綻しなかった。それが本当の批評になった。](https://note.com/moral_spirea2538/)（note）

---

## ファイル構成

| ファイル | 内容 |
|---|---|
| `AIが読む本フォーマット v0_2.md` | 仕様書（v0.2） |
| `AIが読む本フォーマット v0.3.md` | 仕様書（v0.3・現行） |
| `bataille_erotisme_structuration.md` | バタイユへの適用実験（v0.2・誤りを含む） |
| `bataille_erotisme_structuration_v03.md` | バタイユへの適用実験（v0.3・structuring_audit済み） |

---

## v0.3 の追加点

### structuring_audit（新規）

構造化判断を後から検証するための層。構造化エージェントの判断誤りを記録する。

```yaml
structuring_audit:
  - id:
    target:       # 検証対象の content_unit または relation の id
    auditor:      # human / second_ai / etc.
    issue_type:   # rule_misapplication / scope_conflation / agent_bias /
                  # overconfident_breakdown / confirmed
    description:
    correction:
    revised_field:  # optional
```

### internal_contradiction（relations.typeに追加）

同一著者・話者が同一テキスト内で矛盾する主張をしている関係。

### source_text（content_unitsに追加・optional）

原文の該当箇所を保持する。著作権のある既存テキストには適用不可のため任意。

### 設計原則の追加

- 「記述不能」と「検証不能」を区別する
- 構造化判断そのものを検証可能な記録として残す

---

## 基本構造（v0.3）

```yaml
metadata:
  title:
  author:
  genre:
  language:
  narrator:
    specified: true / false
    identity: protagonist / third_party / unknown / omniscient
    note:

structure:
  units:
    - id:
      type:
      title:

content_units:
  - id:
    type:
    location:
    description:      # 記述されていること。解釈を入れない
    source_text:      # optional

relations:
  - id:
    source:
    target:
    type:             # temporal_sequence / explicit_cause / possible_cause /
                      # retrospective_link / contrast / thematic_echo /
                      # assertion_without_evidence / breakdown_echo /
                      # internal_contradiction
    note:

interpretations:
  - id:
    agent:            # narrator / reader / critic
    target:
    claim:
    confidence:       # low / medium / high

uncertainties:
  - id:
    target:
    type:             # causal_unknown / mechanism_unknown / evidence_missing /
                      # witness_unavailable / scope_unknown / interpretation_conflict /
                      # rule_application_ambiguous
    reason:

structuring_audit:
  - id:
    target:
    auditor:
    issue_type:
    description:
    correction:
    revised_field:    # optional
```

---

## 設計原則

- `content_units` には記述されていることだけを入れる。解釈は `interpretations` へ
- 不確定な領域は削除せず `uncertainties` として保存する
- 解釈には必ず主体（`agent`）を付ける
- 因果は確定のものだけでなく候補関係も `possible_cause` として記録する
- 語り部（`narrator`）を明示する。曖昧にする場合もその選択を記録する
- 「記述不能」と「検証不能」を区別する（v0.3追加）
- 構造化判断そのものを `structuring_audit` で検証可能な記録として残す（v0.3追加）

---

## ライセンス

MIT
