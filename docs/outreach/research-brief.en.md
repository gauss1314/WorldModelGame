<!-- Internal: the pre-send checklist and the follow-up interview questions are in research-brief-checklist.md. Delete this line before publishing. -->

# Naturalis (working title): a brief for researchers

Draft 0.2 · [date] · From [Founder name], [email]

> **About the name.** "Naturalis" is a working title only. It is also the name of the Naturalis Biodiversity Center in Leiden, which has no connection with this project. A trademark search is part of Phase 0, and the name may well change.

This brief describes plans, not results.

---

## One-page summary

**What it is.** Naturalis would be a browser-first discovery game set in Lenia, the continuous cellular automaton created by Bert Wang-Chak Chan (Chan, 2019; first posted on arXiv in 2018). Players would be naturalists: exploring rule space, collecting, describing and naming lifeforms, and together writing a permanent, open catalog. Nothing would be invented: every entry would be a pattern that the fixed rules produce.

**Why Lenia.** Chan (2019) described more than 400 species in 18 families, many found by interactive evolutionary computation, with a person choosing in the loop. We want to continue that collaboration between people and algorithms.

**What would be different.**

- *Reproducible, open records.* Each universe would have a versioned spec, never changed once published, and a deterministic integer reference implementation is meant to give bit-identical results on servers and in browsers, so anyone could re-check any record. Catalog data: proposed CC0, with dated public snapshots.
- *Human judgement as data.* We assume automated search out-searches any single player, so an AI census would *collect* specimens where people send it, and people would *describe* them. With consent, what people mark as interesting, or pass over, would become research data.

**Status.** Design stage only: no code, players, funding, partners or legal entity yet. "We" in this brief means the founder alone. If the project goes ahead, a small company or a non-profit (not yet decided) may run it, funded by optional extras such as cosmetics and prints, and by grants; catalog data would stay free. Phase 0 (four weeks of interviews and technical probes) began on [Phase 0 start date].

**What we are asking.** A 30-minute conversation, at a time that suits you. Later, we may also ask whether you would advise: about one hour a month (tentative) on drafts, bounties and census regions. Advice only; decisions stay with the founder. Unpaid at present; you could stop at any time, and would be named publicly only if you agree.

**What researchers could get.** An open-source reference implementation; a human-curated catalog that might serve as a benchmark; later, consented, anonymised data on what people find interesting; bounties co-designed with you.

**Commitments.** Prior work credited, and published species imported only with their authors' permission; naming rights and priority never sold; no tokens or NFTs; nothing staged.

---

## Project description

### The concept

An expedition of 15–40 minutes (tentative) would follow one loop: pick a region of rule space, explore, collect candidates, send them for assay, and write a description. Every expedition would yield something, even when nothing new turns up: it would uncover part of the player's map, add re-sighted species to a personal observation log (like a birder's life list), and produce a field-notebook page. The closest precedents are OEIS and Catagolue: long-running public catalogs of mathematical objects.

### The Lenia multiverse, and why discretisation belongs to the universe

A *universe* would be one fully specified rule: kernel family, growth-function family, β, μ, σ, and the discretisation (kernel radius R, time resolution T and numerical precision). Its spec would never change once published: a mistake would lead to a new version, and old entries would stay valid and runnable. The map players see would be a 2D slice, by default the μ–σ plane for a fixed kernel and resolution. Chan (2019) mapped the distribution of species in parameter space, and Hudcová et al. (2026) propose an automatic method that sorts Lenia systems into four dynamical classes and maps μ–σ phase spaces. We hope to learn from both.

We would treat discretisation as part of the definition because of Davis (2024). As we read the abstract, it describes "non-Platonic" gliders that exist only under particular discretisations, rather than approximating an ideal continuous form. *Orbium* and *Hydrogeminium natans* fit the Platonic criteria, while *Triscutium solidus*, *Scutium gravidus* and a wobble glider in *H. natans* were placed in the non-Platonic category. So "real" should mean the universe exactly as its spec defines it, not an imagined continuum limit. The assay would re-run each species at several values of R and T (precision variants come later) and label it Platonic or non-Platonic in an operational sense (whether it survives across the R and T values tested), which may differ from Davis's own criteria. Both would be valid entries; the label would warn that a species may depend on the discretisation rather than on the continuous rule.

### Determinism

Lenia is sensitive to tiny numerical differences, and floating-point results vary by hardware. Our plan (nothing below is implemented yet): 16-bit integer states (tentative); kernel weights and growth lookup tables generated by integer algorithms fixed in the spec; and exact convolution, so results would be bit-identical everywhere. One Rust codebase would compile to native code and WebAssembly, so browsers could reproduce any entry; GPU float previews would be for exploration only. Growth from random soup is often chaotic and may differ under the integer reference, so the type specimen would be a quantised snapshot of the mature form, re-checked by the reference (we assume, and will test, that mature forms tolerate small numerical differences). Runs would be hash-chained (every 256 steps, tentative) into signed assay certificates, and every engine version archived.

Phase 0 will test internally whether 16-bit states are precise enough, on about ten classic species (tentative; fewer if the founder works alone), starting with *Orbium* (R = 13, T = 10, μ = 0.15, σ ≈ 0.015), using only parameters published in Chan (2019), natively on three operating systems and in three desktop browsers, before any spec is frozen. Results about specific published species will not be released without their authors' agreement.

### Catalog and naming

The rules below are drafts for a naming code to be written in Phase 0; none is final.

- **Collection is not description.** A collection would be the first recorded specimen, from a player or an AI census. A description would be a formal registration: a diagnosis, notes on behaviour, at least one observation, a passed assay and duplicate check, and a declaration of any AI assistance.
- **Priority (proposed).** As in biological nomenclature, the name would belong to the first valid description, timed by a hash commitment at submission. Collector and describer would be credited separately.
- **Names (proposed).** Latinised binomials, globally unique and always chosen by a person (AI would never be a namer), plus common names in any language. After name review and registration, a 30-day objection period (tentative) would follow.
- **Permanent IDs** (for example `LEN-000123`) would never be reused. Entries later found to be the same species would be merged as synonyms: the ID would stay and point to the valid name. Nothing would be deleted.
- **What is a species?** Our draft definition: a region of rule space within which a creature can be carried continuously, moving the parameters step by step without losing it. We would welcome criticism.

**Published species and open data.** Published species would be imported with their original names and attribution, and only with their authors' permission; without it, we would only cite and link them. The data is meant to outlive whoever runs the project: open licences, public snapshots with DOIs, and a yearly test of rebuilding the whole catalog from those snapshots alone.

### Human–AI division of labour

A growing body of work automates discovery in Lenia and related systems (Reinke et al., 2020; Faldor & Cully, 2024; Kumar et al., 2025; Hamon et al., 2025; Michel et al., 2025). Reinke et al. (2020) report that their method was more efficient than several baselines and as efficient as a system pretrained on a hand-made database of human-identified patterns. A larger human-curated, machine-checked catalog might still be useful as a benchmark (how many human-described species can an algorithm recover, and what does it find that people did not?); we would like to test that with researchers rather than assume it. We would not compete on finding. People would stay one level above the search:

- **Directed census.** AI census would run only where people send it. In Phase 1 the founder (with any team by then) would choose regions on advisors' advice and publish the reasons; later, players would propose and vote. Results would go into a pool of undescribed specimens, like museum drawers.
- **"Keen eye" credit.** The first person to flag a specimen as worth describing would be credited if it is later described and cited. Flagging alone would earn nothing.
- **Automation watermark.** Each quarter we would test how much recent human work at each level the AI pipeline can reproduce unaided. Above a threshold (70%, tentative), that level would count for less in reputation and the next level would open. Results would be published; earned honours would keep their value.
- **AI expeditions** (Phase 2). External labs could plug in their own search algorithms; finds would enter as collections credited to the lab.

### Data and consent

Hughes et al. (2024) define open-endedness formally relative to an observer, through novelty and learnability, and argue that it is now achievable "with respect to a human observer". Data on what people find interesting may therefore be useful to that line of research.

Consent would be tiered, defaulting to the most conservative tier, 0, which would record no exploration behaviour beyond the provenance a registration needs. Tier 1 would allow behavioural data to be used to improve recommendations in the game, and tier 2 would also allow it to be anonymised and released in public research datasets.

Consent could be changed or withdrawn at any time. Withdrawal would apply to future releases; anonymised versions already released cannot be recalled, and we would say so up front. By default, no trajectory data would be collected from minors. Before any release we would remove account identifiers, coarsen timestamps, suppress rare trajectories and assess re-identification risk. We intend to seek research ethics review before collecting tier-2 data. We do not yet have an institutional partner for this.

### Roadmap (all durations tentative)

Durations assume a small team of 3–5 people, which does not exist yet. With the founder alone, every phase would take longer.

| Phase | When | Main work |
|---|---|---|
| 0 | W1–W4 (now) | 5–10 interviews; integer reference prototype; determinism tests; minimal explorer; draft spec and naming code; literature-import permission request; name search |
| 1 | about 2 months | Invite-only browser alpha with classic Lenia: assay, duplicate check, registration, literature import (only with the authors' permission; otherwise a citation list), weekly data export, 1–2 seed bounties |
| 2 | months 3–6 | Collision experiments; constructions; formal bounties; multi-channel Lenia (Chan, 2020); first automation watermark review; AI expedition interface |
| 3 | months 6–12 | Flow-Lenia (Plantec et al., 2023); long-running evolution reserves; open API; first curiosity dataset; a data paper with any interested collaborators |
| 4 | after month 12 | More universe families: reaction–diffusion, Life-like rule families, neural cellular automata |

One Phase 1 go/no-go criterion: at least one researcher actually uses the data or co-designs a bounty. If none does, we would stop expanding and return to interviews.

### Open questions we would like your view on

1. **How common are non-Platonic species?** (Phase 1) A census with a discretisation test in every assay could estimate how widespread the gliders reported by Davis (2024) are, and where in rule space.
2. **Do the phase maps generalise?** (Phase 1) We understand that Hudcová et al. (2026) find qualitatively similar μ–σ maps across kernel shapes [verify]. Does this hold in larger rule families?
3. **Are there other collision-robust movers?** (Phase 1) Gupta (2026) reports that, among the rules searched, only *Orbium* kept both individuals intact after a collision [verify]. A judge could collide a mover with its own copy, without a collision interface.
4. **Can a deflected signal be routed onward?** (Phase 2) The same preprint builds an INHIBIT gate from *Orbium* collisions, chains two gates into an AND-NOT chain, and leaves open whether a deflected signal can be routed to a downstream gate [verify]. This would need Phase 2 construction tools.
5. **Mapping and record bounties.** (Phase 2) Where exactly does a "life band" end? What is the smallest stable mover? Which formulations would be scientifically useful to you?

---

## Questions we would love to ask you

Three questions we ask everyone:

1. What would you most like to know about Lenia?
2. What kind of data would you actually use?
3. What kind of result would you be willing to cite, or put your name to?

And, of course: what have we missed, and whose work should we be crediting?

**How we handle the conversation.** We take notes. We will record, quote you or name you publicly only with your explicit permission. Everyone we interview will receive our summary of the open questions we collect; an anonymous version of that list will also be published in the project's public repository. Nothing in either is attributed to anyone who has not agreed.

---

## References

Mentioning a work here does not mean that its authors know of, endorse or are involved in this project. Text in square brackets is our own short description, not the authors' title.

- B. W.-C. Chan, "Lenia: Biology of Artificial Life," *Complex Systems* 28(3), 2019. [doi:10.25088/ComplexSystems.28.3.251](https://doi.org/10.25088/ComplexSystems.28.3.251) · [arXiv:1812.05433](https://arxiv.org/abs/1812.05433)
- B. W.-C. Chan, "Lenia and Expanded Universe," ALIFE 2020. [arXiv:2005.03742](https://arxiv.org/abs/2005.03742)
- B. W.-C. Chan, Lenia source code (MIT licence). [github.com/Chakazul/Lenia](https://github.com/Chakazul/Lenia)
- Plantec et al., ALIFE 2023. [Flow-Lenia: mass conservation and parameter localisation] [arXiv:2212.07906](https://arxiv.org/abs/2212.07906)
- Kumar et al., *Artificial Life* 31(3), 2025. [ASAL: foundation-model search over Lenia and other substrates] [arXiv:2412.17799](https://arxiv.org/abs/2412.17799)
- Reinke, Etcheverry, Oudeyer, ICLR 2020. [IMGEP-based automated discovery in Lenia] [arXiv:1908.06663](https://arxiv.org/abs/1908.06663)
- Faldor & Cully, 2024. [Leniabreeder] [arXiv:2406.04235](https://arxiv.org/abs/2406.04235)
- Hamon et al., *Science Advances*, 2025. [arXiv:2402.10236](https://arxiv.org/abs/2402.10236)
- Michel et al., 2025. [arXiv:2505.15998](https://arxiv.org/abs/2505.15998)
- Q. T. Davis, 2024. [non-Platonic gliders that depend on discretisation] [arXiv:2401.13111](https://arxiv.org/abs/2401.13111)
- Hudcová et al., 2026. [automatic classification of Lenia systems into four dynamical classes; μ–σ phase-space maps] [arXiv:2601.01932](https://arxiv.org/abs/2601.01932) · interactive version linked from the abstract: [lenia-explorer.vercel.app](https://lenia-explorer.vercel.app)
- C. Gupta, 2026, preprint. [collision-based logic gates built from *Orbium*] [arXiv:2609.01348](https://arxiv.org/abs/2609.01348) [verify]
- Hughes et al., ICML 2024, position paper. [open-endedness defined relative to an observer, through novelty and learnability] [arXiv:2406.04268](https://arxiv.org/abs/2406.04268)
- Catagolue, a distributed census of objects in Conway's Game of Life and many other cellular automata, running since 2015. [catagolue.hatsya.com](https://catagolue.hatsya.com/home)
- OEIS, the On-Line Encyclopedia of Integer Sequences. [oeis.org](https://oeis.org)

---

## Contact

**[Founder name]**, Founder, Naturalis (working title)

- Email: [email]
- Website: [website]
- Book a time: [scheduling link]
- Time zone: [time zone]

Thank you for reading. A short reply, even "not now", helps us plan.
