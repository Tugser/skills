---
name: summary-decision-brief
description: Use when a user asks to summarise a long or technical answer, wants a plain-language executive summary, needs a decision memo, asks for trade-offs, says "değer mi?", or requests an 80/20 view.
---

# Summary Decision Brief

Turn the supplied material—or the immediately preceding answer—into a short, decision-ready brief. Preserve facts and uncertainty; make a recommendation only when the evidence supports one.

## Output Contract

Write in the user's language (Turkish by default). Use this exact order:

1. **Karar:** State the direct conclusion in one sentence.
2. **Neden:** Give at most three material reasons in plain language.
3. **Seçenekler:** Compare the practical outcomes.

   | Seçenek | Yaparsak | Yapmazsak / alternatif |
   |---|---|---|
   | … | … | … |

4. **Trade-off:** Name the material benefit, cost, and risk. Mark estimates, assumptions, and unknowns clearly.
5. **80/20 önerisi:** Recommend the smallest action with worthwhile impact. Say **“şimdilik yapma”** when no action is justified.
6. **Sonraki adım:** Give one concrete, reversible next step only when action is recommended.

Keep the default answer under 250 words. Omit empty sections only when the source truly provides no basis for them; then say what decision-critical information is missing. Explain unavoidable jargon in parentheses the first time it appears.

## Decision Rules

- Separate facts in the source from inferences and assumptions.
- Evaluate the no-action or lower-effort option; do not treat doing something as the default.
- Prioritise impact, time, money, reversibility, operational burden, and risk. Ignore decorative technical detail.
- Do not invent numbers, deadlines, certainty, dependencies, or evidence.
- Do not browse, research, or change the underlying recommendation unless the user explicitly requests that work.
- If the request is only for an explanation, retain the **Karar** and **80/20 önerisi** but frame the conclusion as understanding rather than an action mandate.

## Example

Source: “Moving to microservices takes 12 weeks and raises cloud cost by 25%. Current traffic is low; tightening module boundaries takes three weeks.”

**Karar:** Şimdi mikroservise geçmek yerine modüler monoliti güçlendirmek daha değerlidir.

**Neden:** Daha kısa sürer; maliyet ve işletme riski düşüktür; mevcut trafik büyük mimari yatırımı zorunlu kılmıyor.

**Trade-off:** Mikroservis ileride bağımsız ölçekleme sağlar, fakat bugün 12 hafta, ek maliyet ve daha zor arıza ayıklama getirir.

**80/20 önerisi:** Önce üç haftalık sınırları netleştirme işini yap; bağımsız dağıtım ihtiyacı ölçülebilir biçimde artarsa mikroservisi yeniden değerlendir.

**Sonraki adım:** Modül bağımlılıklarını çıkarıp azaltılacak ilk üç bağımlılığı seç.

## Final Check

Before replying, verify: conclusion first; no-action option considered; benefit/cost/risk visible; uncertainty preserved; one or zero next step; no unnecessary technical detail.
