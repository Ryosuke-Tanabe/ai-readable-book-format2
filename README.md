# AIが読む本フォーマット v0.2

AIが文章の主旨を追跡できるようにするための構造化フォーマット。

テキストを `content_units`・`relations`・`interpretations`・`uncertainties` に分解してYAMLとして記述する。v0.2から `narrator`（語り部）フィールドを追加。

→ 背景と発見：[AIが読む本を作ったら、対話の構造的矛盾が見えた](https://note.com/moral_spirea2538/n/n71465bec43f1)（note）
→ 哲学書への適用実験：[バタイユを構造化したら、破綻した。それが批評になった。](https://note.com/moral_spirea2538/n/nd7f5b6afe1da)（note）

---

## ファイル構成

| ファイル | 内容 |
|---|---|
| `AIが読む本フォーマット v0_2.md` | 仕様書・設計思想（v0.2） |
| `bataille_erotisme_structuration.md` | バタイユ「エロティシズム」への適用実験 |

---

## v0.2 の追加点

### narrator フィールド（metadata内）

```yaml
narrator:
  specified: true / false  # 語り部を設定するか否か。false も意図的な選択として記録する
  identity: protagonist / third_party / unknown
  note:                    # 語り部を固定する理由、または意図的に未設定にする理由
```

語り部が誰かによって、同じ構造でも全く別の話になる。`specified: false` は「語り部を意図的に曖昧にする」という選択を記録するためのフィールド。

---

## 基本構造

```yaml
metadata:
  title:
  author:
  genre:
  language:
  narrator:          # optional
    specified:
    identity:
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
    description:     # 記述されていること。解釈を入れない

relations:
  - id:
    source:
    target:
    type:            # temporal_sequence / explicit_cause / possible_cause /
                     # retrospective_link / contrast / thematic_echo

interpretations:
  - id:
    agent:           # narrator / reader / critic
    target:
    claim:
    confidence:      # low / medium / high

uncertainties:
  - id:
    target:
    type:            # causal_unknown / mechanism_unknown / evidence_missing /
                     # witness_unavailable / scope_unknown / interpretation_conflict
    reason:
```

---

## 設計原則

- `content_units` には記述されていることだけを入れる。解釈は `interpretations` へ
- 不確定な領域は削除せず `uncertainties` として保存する
- 解釈には必ず主体（`agent`）を付ける
- 因果は確定のものだけでなく候補関係も `possible_cause` として記録する
- 語り部（`narrator`）を明示する。曖昧にする場合もその選択を記録する

---

## ライセンス

MIT
