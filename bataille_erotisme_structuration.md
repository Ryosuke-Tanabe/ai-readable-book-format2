# バタイユ「エロティシズム」冒頭 — 構造化実験

AIが読む本フォーマット v0.2 を哲学書に適用した実験。
Georges Bataille, *L'Érotisme* (1957) 冒頭部。

関連記事：[バタイユを構造化したら、破綻した。それが批評になった。](https://note.com/moral_spirea2538/n/nd7f5b6afe1da)

目的：`content_rule` の縛りがどこで適用不能になるかを記録する。

---

## 対象テキスト（引用）

> De l'érotisme, il est possible de dire qu'il est l'approbation de la vie jusque dans la mort.
>
> Si l'érotisme, en effet, est une forme de l'activité sexuelle, elle est particulière à l'homme puisqu'elle est une recherche psychologique indépendante de la reproduction. Et cette recherche psychologique, si elle est part d'une exubérance et d'un débordement de vie, est tournée vers la mort.
>
> Nous sommes des êtres distincts, qui naissent et meurent séparés, qui existent séparément. Entre chacun de ces êtres, il y a une distance infranchissable : une discontinuité. Cette discontinuité fait de nous des êtres distincts, des individus. Cette discontinuité ne cesse que lors de notre mort.
>
> Il y a ainsi, dans la reproduction et la mort, des passages de la continuité à la discontinuité. Et chez les hommes existe ainsi une nostalgie de la continuité perdue.
>
> Ce passage du discontinu au continu met nécessairement en jeu l'être en entier et se fait ainsi par une certaine forme de violence.

---

## 構造化

```yaml
metadata:
  work_id: bataille_erotisme_opening
  title: L'Érotisme（冒頭部）
  author: Georges Bataille
  genre: philosophy
  mode: nonfiction
  language: fr
  narrator:
    specified: true
    identity: third_party
    note: 著者は外部の観察者として普遍的主張を行う語り口を取る。ただしその普遍性の根拠は提示されない。

format_rules:
  content_rule: "description must describe what is stated, observed, or explicitly reported, not inferred meaning"
  structural_note: "content_ruleの適用不能箇所はbreakdown_pointとして記録する"
  confidence_scale:
    allowed_values: [low, medium, high]
  relation_types:
    allowed_values:
      - temporal_sequence
      - explicit_cause
      - possible_cause
      - retrospective_link
      - contrast
      - thematic_echo
      - assertion_without_evidence

structure:
  units:
    - id: s1
      type: section
      title: 定義
    - id: s2
      type: section
      title: 不連続性の主張
    - id: s3
      type: section
      title: 暴力への接続

content_units:
  - id: ev1
    type: definition
    location: s1
    description: author_defines_eroticism_as_approbation_of_life_unto_death
    breakdown_point: true
    breakdown_reason: >
      "approbation（肯定・是認）"は評価的概念であり観察可能な事実ではない。
      定義の中に解釈が埋め込まれており、content_ruleの「述べられていること」と
      「推論された意味」の分離が定義文の時点で不可能。

  - id: ev2
    type: claim
    location: s1
    description: author_claims_eroticism_is_psychological_search_independent_of_reproduction
    breakdown_point: true
    breakdown_reason: >
      「心理的探求」が何を指すか定義されていない。
      「生殖から独立している」という主張の根拠が提示されていない。
      観察ではなく断言。

  - id: ev3
    type: claim
    location: s2
    description: author_asserts_all_humans_are_discontinuous_beings_separated_at_birth_and_death

  - id: ev4
    type: claim
    location: s2
    description: author_asserts_discontinuity_ceases_only_at_death
    breakdown_point: true
    breakdown_reason: >
      死において不連続性が「消滅する」という主張は検証不能。
      死後の状態について何が観察可能かが示されていない。

  - id: ev5
    type: claim
    location: s2
    description: author_attributes_nostalgia_for_lost_continuity_to_all_humans
    breakdown_point: true
    breakdown_reason: >
      「失われた連続性への郷愁」を全人類に帰属させているが、
      誰の観察か、どのような証拠に基づくかが一切示されていない。
      content_ruleでは記述不能。interpretationsに移動が必要だが、
      著者は事実として述べているため構造的矛盾が生じる。

  - id: ev6
    type: claim
    location: s3
    description: author_asserts_passage_from_discontinuous_to_continuous_necessarily_involves_violence
    breakdown_point: true
    breakdown_reason: >
      "nécessairement（必然的に）"という語が根拠なく使用されている。
      暴力の定義も提示されていない。
      論理的必然として述べられているが、論証が存在しない。

relations:
  - id: rel1
    source: ev1
    target: ev2
    type: assertion_without_evidence

  - id: rel2
    source: ev3
    target: ev4
    type: assertion_without_evidence

  - id: rel3
    source: ev4
    target: ev5
    type: assertion_without_evidence

  - id: rel4
    source: ev5
    target: ev6
    type: assertion_without_evidence

interpretations:
  - id: int1
    agent: reader
    target: ev1
    claim: approbation_may_be_metaphorical_rather_than_literal_but_distinction_is_not_made
    confidence: low

  - id: int2
    agent: reader
    target: ev5
    claim: nostalgia_for_continuity_may_be_authors_personal_experience_generalized_to_universal
    confidence: medium

  - id: int3
    agent: critic
    target: ev6
    claim: violence_may_function_rhetorically_to_intensify_argument_rather_than_as_defined_concept
    confidence: medium

  - id: int4
    agent: critic
    target: ev1
    claim: the_definition_is_itself_the_argument_making_the_claim_unfalsifiable
    confidence: high

uncertainties:
  - id: un1
    target: ev1
    type: scope_unknown
    reason:
      - approbation_undefined
      - life_and_death_as_used_here_are_not_defined
      - whether_this_is_descriptive_or_normative_is_unclear

  - id: un2
    target: ev2
    type: evidence_missing
    reason:
      - no_empirical_evidence_for_independence_from_reproduction
      - psychological_search_undefined

  - id: un3
    target: ev4
    type: mechanism_unknown
    reason:
      - state_after_death_is_not_observable
      - continuity_itself_is_not_defined

  - id: un4
    target: ev5
    type: evidence_missing
    reason:
      - no_source_or_evidence_for_universal_human_nostalgia
      - lost_continuity_presupposes_prior_continuity_which_is_not_demonstrated

  - id: un5
    target: ev6
    type: causal_unknown
    reason:
      - necessity_of_violence_is_asserted_not_demonstrated
      - violence_undefined

breakdown_summary:
  total_content_units: 6
  breakdown_points: 5
  breakdown_rate: "83%"
  note: >
    6つのcontent_unitsのうち5つでcontent_ruleの適用が困難または不可能。
    「述べられていること」と「推論・評価・断言」の分離が冒頭定義文から成立しない。
    全てのrelationがassertion_without_evidenceになった。
    uncertaintiesが5つ生成されたが、これは記述できた範囲であり、
    実際にはev1の時点で分析の前提が崩れている。
```
