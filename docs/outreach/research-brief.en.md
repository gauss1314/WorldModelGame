<!-- ===== 内部检查清单开始 ===== -->

> ## 发送前检查（发送时删除本节）
>
> 本简介是第 0 阶段第 1 周（W1）发给研究者的第一份材料。GDD 是内部文档，不对外发送。发送前逐项确认：
>
> 1. **占位符**：全文搜索 "["，填好 [Founder name]、[email]、[website]、[scheduling link]、[time zone]。没有网站就删掉那一行，不要留空链接。
> 2. **[verify] 标注**：下面这些说法只来自摘要或二手摘要。发送前读原文核实；核实不了就删掉整句，不要只删标记。
>    - Gupta（2026）：INHIBIT 门、AND-NOT 链、"信号路由留作开放问题"、"只有 Orbium 碰撞后双方完整"。目前只有二手摘要。
>    - Hudcová 等（2026）："不同核形状下相图定性相似"。四类动力学和 μ–σ 相图也只读过摘要。
>    - Davis（2024）：哪些物种属于柏拉图型、哪些属于非柏拉图型，正文已写"as we read the abstract"，仍需对照原文。
>    - 参考文献方括号里的说明是我们写的，不是论文标题。如果要换成正式标题，逐条核对。
>    - 发给上述作者本人时，涉及对方工作的句子先改成提问，或请对方确认。
> 3. **名字**：对外只写 "Naturalis (working title)"。商标检索有结论或已改名时，更新标题和文首说明。不能暗示与 Naturalis Biodiversity Center 有任何关系。
> 4. **数据许可**：§22(6) 是否已经决定？未决定就保留 "proposed CC0"，不要写成已定。参考实现用 MIT 还是 Apache-2.0 同理，正文目前没写具体许可。
> 5. **文献导入许可**：本简介不是许可请求。发给 Chan 时另附许可请求函，并核对两份材料口径一致：只在获得许可后导入；保留原作者和年份；物种数据能否以 CC0 再发布尚未获准。
> 6. **项目状态**：发送当天是否仍然是"只有设计文档，没有代码、团队、玩家、资金和合作方"？有变化就如实更新（例如原型已经能复现 Orbium），但不要夸大。不要提到任何尚未答应的人。
> 7. **顾问条件**：每月约 1 小时（暂定）、目前不付报酬、可随时退出、公开列名须本人同意。这些要和创始人的决定一致。
> 8. **访谈承诺**：正文承诺了三件事：录音、引用和公开署名都先征得同意；访谈后把开放问题汇总发给所有受访者。确认能做到再发。
> 9. **伦理审查**：正文写的是"计划在收集 2 级数据之前寻求伦理审查，目前没有合作机构"。找到合作机构后再改写。
> 10. **按收件人定制**：
>     - 开头加一两句：为什么联系对方，读过对方的哪项工作。不要群发，不要互相抄送。
>     - 把"开放问题"里和对方最相关的一两项挪到前面：Chan（文献导入、署名、命名）；Inria Flowers 团队、ASAL 作者、Leniabreeder 作者（基准、AI 探险队接口、好奇心数据）；Lenia 参数空间可视化的作者（宇宙图底图、相图问题）；Catagolue 维护者（普查、志愿算力、目录的组织方式）。
>     - 不要暗示被引用的作者或其他收件人已经认可或参与本项目。
> 11. **篇幅**：一页摘要可以直接作为邮件正文，其余部分作为附件或链接。
>
> （检查清单到此结束，发送时连同本行一起删除。）

<!-- ===== 内部检查清单结束 ===== -->

# Naturalis (working title): a brief for researchers

Draft 0.1 · Phase 0, Week 1 (W1) · From [Founder name], [email]

> **About the name.** "Naturalis" is a working title only. It is also the name of the Naturalis Biodiversity Center in Leiden, which has no connection with this project. A trademark search is part of Phase 0, and the name may well change.

This brief describes plans, not results.

---

## One-page summary

**What it is.** Naturalis is a browser-first discovery game set in Lenia, the continuous cellular automaton introduced by Chan (2019). Players act as naturalists. They explore rule space, collect lifeforms, describe and name them, and study how they behave. Together they write a permanent, open catalog. Nothing in it is invented: once the rules are fixed, the lifeforms already exist mathematically, waiting to be seen.

**Why Lenia.** Lenia is visually rich, well documented and still open. Chan (2019) described more than 400 species in 18 families, many found by interactive evolutionary computation, with a person choosing in the loop. We want to continue that collaboration between people and algorithms. Later phases add multi-channel Lenia, where Chan (2020) reports self-replication, and Flow-Lenia (Plantec et al., 2023).

**What is different.**

- *Rigour.* Each universe would be defined by a versioned spec that never changes once published. A mistake leads to a new version; old entries stay valid and runnable. A deterministic integer reference implementation is meant to give bit-identical results on servers and in the browser. Every record would carry machine-verified provenance: spec version, parameters, a type specimen, and a chain of state hashes that anyone can re-run and check.
- *Open data.* Catalog data under a proposed CC0 licence, with dated public snapshots.
- *Human judgement as data.* Automated search is strong, and we assume it out-searches any single player from day one. So we separate *collection*, which an AI census can do, from *description*, which people do. People decide where the census looks, and they are credited for spotting what is worth describing. With consent, what people mark as interesting, and what they pass over, becomes a research dataset. A quarterly "automation watermark" review checks which human contributions AI can now reproduce, and moves human roles one level up.

**Status.** Design stage only. There is an internal design document, but no code, no team beyond the founder, no players, no funding and no partners. Phase 0, four weeks of interviews and technical probes, starts now.

**What we are asking.** A 30-minute conversation, at a time that suits you. If there is a good fit, we would also like to invite at least two researchers to act as advisors, for about one hour a month (tentative): commenting on drafts and helping choose the first bounties. We have no funding, so we cannot pay advisors at present.

**What researchers could get.**

- Open catalog data under a proposed CC0 licence.
- An open-source, deterministic reference implementation.
- A human-curated catalog that could serve as a benchmark for automated discovery.
- Later, consented and anonymised data on what people find interesting.
- Bounties co-designed with you, with the question, the automatic judge and the credit agreed before launch.

**Our commitments.**

- We credit prior work. Published species would be imported with their original attribution, and only with the authors' permission.
- Naming rights and priority are never sold.
- No tokens and no NFTs.
- No staged discoveries: everything we show comes from the rules.
- The data is meant to outlive the company: open licences, public snapshots with DOIs, and a yearly test of rebuilding the whole catalog from those snapshots alone.

---

## Project description

### The concept

An expedition of 15–40 minutes (tentative) follows one loop: pick a region of rule space, explore, collect candidates, send them for assay, and write a description. Every expedition yields something, even when nothing new turns up: it uncovers part of the player's map, adds re-sighted species to a personal observation log (like a birder's life list), and produces a field-notebook page. The closest precedents are OEIS and Catagolue: permanent, citable catalogs of mathematical objects.

### The Lenia multiverse, and why discretisation belongs to the universe

A *universe* is one fully specified rule: kernel family, growth-function family, β, μ, σ, and the discretisation (kernel radius R, time resolution T and numerical precision). The map players see is a 2D slice, by default the μ–σ plane for a fixed kernel and resolution. Chan (2019) mapped the distribution of species in parameter space, and a two-page abstract by Hudcová et al. (2026) reports automatic classification of Lenia systems into four dynamical classes, with μ–σ phase-space maps. We hope to learn from both.

We treat discretisation as part of the definition because of Davis (2024). As we read the abstract, it describes "non-Platonic" gliders that exist only under particular discretisations, rather than approximating an ideal continuous form. *Orbium* and *Hydrogeminium natans* fit the Platonic criteria, while *Triscutium solidus*, *Scutium gravidus* and a wobble glider in *H. natans* were placed in the non-Platonic category. So "real" should mean the universe exactly as its spec defines it, not an imagined continuum limit. The assay would re-run each species at several values of R and T (precision variants come later) and label it Platonic or non-Platonic. Both are valid entries; the label warns that a species may depend on the discretisation rather than on the continuous rule.

### Determinism

Lenia is sensitive to tiny numerical differences, and floating-point results differ across hardware, so two machines can diverge within a few thousand steps. Our plan:

- **Integer semantics.** States are 16-bit integers (tentative). Kernel weights are generated from the parameters by an algorithm fixed in the spec, as non-negative integers summing to 2^16 (tentative). Convolution is exact: direct summation or a number-theoretic transform, with identical bits either way. Growth uses an integer lookup table generated the same way. Both are hashed into each record.
- **One codebase, bit-exact everywhere.** The reference is written in Rust and compiled to native server code and to WebAssembly, so a browser can reproduce any entry exactly. Fast GPU float previews are for exploration only.
- **Type specimen = mature form.** Growth from random soup is often chaotic and may not reproduce exactly, but a grown creature is an attractor and tolerates tiny differences. So the type specimen is a quantised snapshot of the mature form; the soup and exploration path are kept as provenance.
- **Hash chains and archives.** The reference run hashes the state every 256 steps (tentative) and chains the hashes into a signed assay certificate. Every engine version is archived.

We do not yet know whether 16-bit states are precise enough. Phase 0 tests this on about 10 classic species (tentative), starting with *Orbium* (R = 13, T = 10, μ = 0.15, σ ≈ 0.015), across three platforms, before any spec is frozen.

### Catalog and naming

- **Collection is not description.** A collection is the first recorded specimen, from a player or an AI census. A description is a formal registration: a diagnosis, notes on behaviour, at least one observation, a passed assay and duplicate check, and a declaration of any AI assistance.
- **Priority.** As in biological nomenclature, the name belongs to the first valid description, timed by a hash commitment at submission. Collector and describer are credited separately.
- **Names.** Latinised binomials, globally unique and always chosen by a person (AI is never a namer), plus common names in any language. After name review and registration, a 30-day objection period (tentative) follows.
- **Permanent IDs** (for example `LEN-000123`) are never reused. Entries later found to be the same species are merged as synonyms: the ID stays and points to the valid name. Nothing is deleted.
- **What is a species?** Our draft definition: a region of rule space within which a creature can be carried continuously, moving the parameters step by step without losing it. We would welcome criticism.

### Human–AI division of labour

A growing body of work automates discovery in Lenia and related systems (Reinke et al., 2020; Faldor & Cully, 2024; Kumar et al., 2025; Hamon et al., 2025; Michel et al., 2025). Reinke et al. compared their method with a system pretrained on a hand-made database of human-identified patterns; a human-curated, machine-checked catalog could play that reference role at scale. We do not compete on finding. People stay one level above the search:

- **Directed census.** AI census runs only where people send it. In Phase 1 the team and advisors choose regions and publish the reasons; later, players propose and vote. Results go into a pool of undescribed specimens, like museum drawers.
- **"Keen eye" credit.** The first person to flag a specimen as worth describing is credited if it is later described and cited. Flagging alone earns nothing.
- **Automation watermark.** Each quarter we test how much recent human work at each level the AI pipeline can reproduce unaided. Above a threshold (70%, tentative), that level counts for less in reputation and the next level opens. Results are published; earned honours keep their value.
- **AI expeditions** (Phase 2). External labs could plug in their own search algorithms; finds would enter as collections credited to the lab.

### Data and consent

Hughes et al. (2024) define open-endedness relative to a human observer, so data on what people find interesting may be useful to that line of research. It needs tiered consent, defaulting to the most conservative tier:

| Tier | What is recorded and how it is used |
|---|---|
| 0 (default) | No behavioural logging. Only the provenance a registration needs. |
| 1 | Used to improve recommendations in the game. |
| 2 | Anonymised and released in public research datasets. |

Consent can be changed or withdrawn at any time. Withdrawal applies to future releases; anonymised versions already released cannot be recalled, and we say so up front. By default, no trajectory data is collected from minors. Before any release we remove account identifiers, coarsen timestamps, suppress rare trajectories and assess re-identification risk. We intend to seek research ethics review before collecting tier-2 data. We do not yet have an institutional partner for this.

### Roadmap (all durations tentative)

| Phase | When | Main work |
|---|---|---|
| 0 | W1–W4 (now) | 5–10 interviews; integer reference prototype; determinism tests; minimal explorer; draft spec and naming code; literature permission; name search |
| 1 | about 2 months | Invite-only browser alpha with classic Lenia: assay, duplicate check, registration, literature import, weekly data export, 1–2 seed bounties |
| 2 | months 3–6 | Collision experiments; constructions; formal bounties; multi-channel Lenia; first automation watermark review; AI expedition interface |
| 3 | months 6–12 | Flow-Lenia; long-running evolution reserves; open API; first curiosity dataset; a data paper with any interested collaborators |
| 4 | after month 12 | More universe families: reaction–diffusion, Life-like rule families, neural cellular automata |

One Phase 1 go/no-go criterion: at least one researcher actually uses the data or co-designs a bounty. If none does, we stop expanding and return to interviews.

### Open questions we would like your view on

1. **How common are non-Platonic species?** (Phase 1) A census with a discretisation test in every assay could estimate how widespread the gliders reported by Davis (2024) are, and where in rule space.
2. **Do the phase maps generalise?** (Phase 1) We understand that Hudcová et al. (2026) find qualitatively similar μ–σ maps across kernel shapes [verify]. Does this hold in larger rule families?
3. **Are there other collision-robust movers?** (Phase 1) Gupta (2026) reports that, among the rules searched, only *Orbium* kept both individuals intact after a collision [verify]. A judge can collide a mover with its own copy, without a collision interface.
4. **Can a deflected signal be routed onward?** (Phase 2) The same preprint builds an INHIBIT gate from *Orbium* collisions, chains two gates into an AND-NOT chain, and leaves open whether a deflected signal can be routed to a downstream gate [verify]. This needs Phase 2 construction tools.
5. **Mapping and record bounties.** (Phase 2) Where exactly does a "life band" end? What is the smallest stable mover? Which formulations would be scientifically useful to you?

---

## Questions we would love to ask you

Three questions we ask everyone:

1. What would you most like to know about Lenia?
2. What kind of data would you actually use?
3. What kind of result would you be willing to cite, or put your name to?

And some follow-ups:

4. Which fields and measurements would a catalog record need for your work, and in what format?
5. What reproducibility evidence would you need before citing a record? For example: an independent re-run, a hash chain, an archived engine version, or a published spec.
6. Is it acceptable to you to treat one specific integer discretisation as "the" universe? Would a Platonic / non-Platonic label be useful, or misleading?
7. Would a human-curated catalog be useful as a benchmark for automated discovery? What task and measure would make it worth using?

And, of course: what have we missed, and whose work should we be crediting?

**How we handle the conversation.** We take notes. We record only with your permission. We will not quote you or name you publicly without your explicit permission. Everyone we interview will receive our summary of the open questions we collect.

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
- Hudcová et al., 2026, two-page abstract. [classification of Lenia systems into four dynamical classes; μ–σ phase-space maps] [arXiv:2601.01932](https://arxiv.org/abs/2601.01932) · interactive version: [lenia-explorer.vercel.app](https://lenia-explorer.vercel.app)
- C. Gupta, 2026, preprint. [collision-based logic gates built from *Orbium*] [arXiv:2609.01348](https://arxiv.org/abs/2609.01348) [verify]
- Hughes et al., ICML 2024, position paper. [open-endedness defined relative to a human observer] [arXiv:2406.04268](https://arxiv.org/abs/2406.04268)
- Catagolue, the Game of Life census (running since 2015). [catagolue.hatsya.com](https://catagolue.hatsya.com/home)
- OEIS, the On-Line Encyclopedia of Integer Sequences. [oeis.org](https://oeis.org)

---

## Contact

**[Founder name]**, Founder, Naturalis (working title)

- Email: [email]
- Website: [website]
- Book a time: [scheduling link]
- Time zone: [time zone]

Thank you for reading. A short reply, even "not now", helps us plan.
