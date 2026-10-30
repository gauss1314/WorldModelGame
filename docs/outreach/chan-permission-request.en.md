# Literature-import permission request to Bert Wang-Chak Chan（文献物种导入许可请求）

> 状态：草稿 v0.2 · 2026-10-20（已按对抗式审查修订） · **未发送** · 依据 GDD v0.2 §3.1(5)、§8.5(9)、§14.5、§14.9、§17、§19.1、§22(6)
>
> 相关：[决策日志](../decision-log.md) D-018（数据许可）· [第 0 阶段执行计划](../phase0-plan.md) · [研究者外联简介](./research-brief.en.md)
>
> 本文件分三部分：内部说明（中文）、邮件正文（英文）、可能的回复与对应做法（中文）。**只发送第二部分的主题和正文。**

---

## 内部说明（发送时删除）

### 1. 目的

- 取得一份**书面许可**：把 Chan 2019 年论文（*Complex Systems* 28(3)，doi:10.25088/ComplexSystems.28.3.251）和代码仓库 github.com/Chakazul/Lenia 里的物种参数和模式数据，作为"文献记录"导入开放图鉴。这是第 0 阶段的交付"确认文献物种的导入许可"（§19.1）。
- 弄清三件事：再发布用 CC0 还是 CC BY；对方希望怎样署名；原名和分类是否要原样保留。
- 发出参与邀请。§17 要求尽早联系 Lenia 的提出者，说明项目、邀请参与，避免让人觉得我们在"占领"别人开辟的领域。
- **为什么代码是 MIT 也要问**：MIT 通常用于代码，它是否覆盖仓库里的物种数据文件不清楚；即使覆盖，MIT 要求所有副本保留版权和许可声明，我们不能自行把这些数据改成 CC0 发布。GDD §3.1(5) 也写明导入前应征得作者同意。法律上是否必须征得同意不是重点，重点是尊重原作者。
- **这封信不是**：访谈邀请、顾问合同或宣传。信里不声称以前有过任何联系。

### 2. 数据许可方向（D-018 已采纳）

邮件 (b) 会问对方 CC0 是否可以、是否更希望 CC BY。§22(6) 已由 D-018 定为 A（2026-10-30），下面两件按它执行，免得承诺了做不到的事。

**第一件：我们向对方提出的"当前方案"。** 默认按 §22(6) 的默认值：图鉴数据 CC0，文字和图版 CC BY 4.0（§14.9）。邮件里写成"当前方案"（邮件正文不随拍板改动）。

**第二件：对方更希望 CC BY 时，我们接受哪种做法。** 选项名称和决策日志 D-018 一致（D-018 的 A 是"图鉴数据 CC0，文字和图版 CC BY 4.0"）：

| 选项 | 做法 | 代价 |
|---|---|---|
| A′ | 在 A 的基础上，只有从 Chan 的工作导入的条目用 CC BY 4.0，其余数据仍是 CC0 | 图鉴变成混合许可；导出要按条目标许可（§14.5 已有 `license` 字段）；§2.2、§14.9 里"图鉴数据用 CC0"要加上"文献导入部分除外" |
| B | 整个图鉴数据改用 CC BY 4.0 | 许可统一，但所有数据的再利用门槛都提高 |
| A″ | 对方说数据按 MIT 处理 | 导入条目的 `license` 标 MIT，附 Chan 的版权声明和 MIT 全文，导出时一并附上；和 A′ 一样是混合许可；MIT 用在数据上是否合适，列入法律备忘 |
| 不导入 | 只引用，不导入数据 | 见第 6 节的退路 |

**按 D-018：文献导入按作者意愿，对方更希望 CC BY 时用 A′，要求 MIT 时用 A″**；不选 B。对方的偏好只约束来自对方的数据，不必因此改变整本图鉴；数据模型已经支持按条目标许可。另请法律顾问看一个问题：玩家从文献种"从这里出发"找到的新种，算不算 CC BY 数据的衍生物（法律备忘 Q14）。

### 3. 发送前检查清单

- [ ] 第 2 节的两件事已经决定
- [ ] 对外承诺 D-040 已拍板（W1 第 2 天）；邮件里做不到的承诺已经删掉，不先发后改
- [ ] 研究简介已定稿，链接能打开；简介里把文献导入写成"计划中，待作者许可"，不写成已经做了
- [ ] 替换全部占位符：[Founder name]、[email]、[website]、[research brief link]
- [ ] 核实仓库里哪些文件含 2019 年论文的物种参数和模式，记下路径，填进第 5 节的确认句 [verify]
- [ ] 对方的邮箱从公开来源核实（论文的作者信息或 GitHub 主页），记下来源；不猜测地址
- [ ] 称呼用 "Dear Bert Chan,"，不猜测头衔
- [ ] 如果 D-013 决定改名或改用代号，发信前把主题和正文里的 "Naturalis (working title)" 全部替换
- [ ] 项目名写作 "Naturalis (working title)"，不把名字说成定名（与荷兰莱顿的 Naturalis Biodiversity Center 同名，§22(1)）
- [ ] 正文没有声称已有团队、资金、玩家、合作方或以前的联系
- [ ] 单独发送，不抄送其他研究者
- [ ] 不另给 Chan 发访谈邀请。(d) 已包含参与邀请；如果需要访谈，在对方回复后再顺带提
- [x] 第 6 条"用这些参数的随机汤"包括离已发表参数很近的格点（D-049，选项 A）：服务器的地形、普查和新手区都不进文献邻域，底图只覆盖地形区（普查 CA4）；邮件不用改

### 4. 发送时机与跟进

| 时间 | 动作 |
|---|---|
| **第 1 周（W1）** | W1 第 3 天，研究简介定稿当天发出（D-040 拍板之后；D-018 已采纳）。和其他研究者的外联同一天开始，但这封单独发 |
| 发出后约 10 天（暂定），即第 2 周末或第 3 周初 | 没有回复的话，发**一次**简短的跟进（模板见下） |
| **第 4 周（W4）末** | 仍没有回复：在私密记录里记为"无回复"；决策日志按 D-042(e) 只写"文献物种：按退路处理，见 §19.1 修订"。第一阶段按第 6 节的退路规划。之后不再就此事追问 |

**W4 末之后收到的答复照样处理**，按"可能的回复"表执行：同意的话，退路可以升级为导入，同步更新 GDD §19.1 和第一阶段 PRD；反对内部使用的话，立即停用（第 0 阶段计划 §5.3）。我们自己不再追问。

如果以后有实质上不同的新请求（例如第二阶段要导入 Chan 2020 年《Lenia and Expanded Universe》里的多通道物种），可以作为新请求另发，但要先在决策日志里单独决定。

**跟进模板**（英文，发送时只保留这几行）：

> Subject: Re: Permission request: Lenia species in an open catalog (Naturalis, working title)
>
> Dear Bert Chan,
>
> I am writing once more about my message of [date], in case it did not reach you. There is no pressure at all: a one-line "no" is completely fine, and in that case we would copy none of your data and only cite your paper. I am happy to answer any questions; the brief is here: [research brief link].
>
> With best wishes,
> [Founder name]

**与第 0 阶段测试的关系**：第 0 阶段要用约 10 个经典物种（如 Orbium）测试整数参考实现。这是内部测试，用的是公开发表的参数，不对外发布数据，所以不必等这封信的答复。但测试结果中涉及具体物种在 lenia-q16 下能否复现的部分，等答复之后再决定是否公开。邮件里已经说明了这种内部测试（"A few things you should know" 第二条）。如果对方的答复连这种内部使用也反对，就停下来，改用我们自己找到的模式做测试。只用论文公开发表的参数，不用仓库里的模式数据，也不加载 Chan 程序自带的物种库（第 0 阶段计划 §5.3，D-040 第 6 条）。"内部测试不必等答复"是我们的判断，不是法律结论 [待法务确认]。

### 5. 什么算许可

- **只认书面答复**：Chan 本人的邮件回复，或其他可以存档的书面形式。
- **各项分开看**：同意 (a) 导入，不等于同意 (b) 的 CC0。许可方向必须在答复里明确写出；没写就回信确认一次，确认之前不公开发布导入的数据。
- **"代码是 MIT 的，随便用"**：视为同意 (a)。但 MIT 要求副本保留版权和许可声明，和 CC0 不一样，所以 (b) 仍要确认一次。
- **口头、视频通话或社交媒体上的同意**：用邮件复述要点，对方书面确认之后才算。
- **不算许可**：沉默、已读、自动回复、点赞。
- **有条件的同意**：许可范围以答复的文字为准，只答应了一部分就只导入那一部分。
- **他人的权利**：如果对方说部分物种或数据另有作者或权利人，那一部分要另外取得许可。论文正文和图的版权可能属于期刊出版方 [verify]：只从仓库取参数和模式，不复制论文的图和正文，图版由我们从数据重新生成（文献种默认不出图版，条件见[纸面生成器](../ux/paper-generator.md) PG13）。
- **确认句**：对方同意 (a)(b) 之后，回信附一句话，请对方回复 "yes"："To confirm: to the extent you hold rights in the parameters and patterns of the species described in Chan (2019), as found in [repository file paths, verify], you dedicate them to the public domain under CC0 1.0 [or: license them to everyone under CC BY 4.0, credited as "..."] [or: license them to everyone under the MIT licence, with your copyright notice kept with every copy], and you are happy for Naturalis (working title) to include them in its catalog. A one-word "yes" is enough."（措辞与法律备忘 §4.2 第 0 点一致；用 "to the extent you hold rights"，不要求对方声明自己是唯一的权利人。） 放弃或许可要由权利人本人作出，不是同意本项目代为发布（法律备忘 §4.2）[待法务确认]。收到 "yes" 之前不公开发布导入的数据。

### 6. 退路：没有回复，或答复是"不"

**做法（暂定）**：

1. **只引用和链接。** 不导入参数和模式数据，不再发布论文或仓库里的任何数据文件。图鉴设一个"文献"页，列出论文和代码仓库的链接。
2. **按需建文献名条目，保留文献优先权。** 不一次性批量列出论文里全部 400 多个名字和科。评审确认某个玩家的发现对应论文里的某个种时，才建一条**文献引用条目**：学名（按原文的双名）、原作者署名、原文链接，不附参数和模式标本。状态建议新增 `literature_reference`（按 D-042(a) 占用 LEN 编号）。批量列出名字和科是否可行，列入法律备忘 [待法务确认]；即使可行，对方明确拒绝时也不批量列出。
3. **让玩家自己重新找到。** 玩家在探索中碰到已知种是正常的。经评审确认后，这次发现登记为该文献种的**再发现**：名字仍然是 Chan 的，玩家记采集者的功劳，不获得命名权。玩家的成体快照可以作为这个种在 lenia-q16 中的**参考标本**，但不是这个名字的模式标本，以免改写原作者的分类；没有许可时，条目不写参考标本与原文参数的差别（命名法规 v0 第 44 条第 3 款）。
4. **团队不按论文参数系统地重建整张物种表。** 那样做实质上是绕开了对方的拒绝。也不用悬赏、考察季、新手考察区或 AI 普查去专门复原文献种。
5. **教程（§3.3(4)）**：原先"带到 Orbium 附近自己找"的做法已被 D-050 取代：没有许可时，教程去团队找到的教程点，不出现文献种，也不进任何文献邻域（[界面线框](../ux/wireframes-first-session.md) UX7、采集交接第 12 节、PRD F13）；只有对方明确允许引用已发表的参数时，才按论文参数在玩家眼前长出 Orbium。教程是否用 Chan 的名字，按 (c) 的答复；获得同意之前用中性的工作名。这是内部说明，邮件正文不改。
6. **对外措辞**：文献条目页写"本图鉴未收录该物种的原始数据，详见原文"。不对外说"作者拒绝了"，也不替对方解释原因。

**这个退路和 GDD 的诚实目标是否一致？方向一致，但查重会变弱，要如实写出。**

- **一致的地方**：红线 1（不伪造发现）不受影响。§3.1(5) 和 §17 要求尊重先行工作、署原作者的名，列出名字并保留优先权做到了这一点。§8.5(9) 的署名格式不变。尊重对方的意愿本身就是学术诚信的一部分。
- **变弱的地方**：
  1. **查重**：§8.4 的自动查重要有模式标本才能比对。没有文献标本，机器认不出"重新找到的文献种"。§3.1(5) 说导入文献是为了让"新物种"三个字诚实；退路下这一点只能靠人工评审对照论文的图和公开参数来保证。对策：参数落在论文已发表物种附近的候选一律送人工评审。§18.1 的"错当新种 < 5%"也只能人工抽检。
  2. **模式标本**：§8.5(2) 要求每个种有且只有一个模式标本，只有名字的文献条目做不到。命名法规 v0 需要写明：已发表的文献名不依赖本图鉴的标本即可成立并享有优先权（这和生物命名法规里已发表名称的效力一致）。
  3. **路线图**：§19.1 把"文献导入"列为第一阶段不能砍的项目，但没有写获不得许可时怎么办。退路下它变成"文献引用表"。GDD 和第一阶段 PRD 要写明这个替代方案，不能把"文献引用表"说成"已导入文献"。

### 7. 结果记在哪里

- 逐项结果只记在私密位置 [存档位置]，不进仓库；本节的表只是模板。原始邮件也存放在那里，不贴原文，不写对方的邮箱。
- 公开的决策日志 `docs/decision-log.md` 只写对外可见的结果，例如"文献物种：按退路处理，见 §19.1 修订"或"文献物种：已获作者许可导入，许可为 CC0"，不写对方答复的内容，也不写"不同意"。
- 对外写"经作者许可导入"之前，先在确认信里问对方是否同意这样写。
- **邮件里的承诺**写进决策日志，作为命名法规 v0、文献映射方案和第一阶段 PRD 的约束：(1) 不为了让文献种在 lenia-q16 中存活而调参；(2) 文献种给任何人（包括内测玩家）看之前，先给对方预览；(3) 原名和原分类在条目上始终可见，修订留痕；(4) 获准之前不公开内部测试中涉及文献种的结果；(5) 对方要求时停止内部使用；(6) 内部测试只按 2019 论文发表的参数运行约 10 个物种（如 Orbium），以及它们改变 R、T 的变体和用这些参数的随机汤，不用仓库数据文件，也不加载 Chan 程序自带的物种库；(7) 没有对方的明确同意，不把新种放进对方的属，也不把对方的种改到别的属（即 D-040 第 1–6 条和第 9 条）。
- 需要同步修改的文档：GDD §14.5 的 `license` 字段和 §8.5(9) 的署名示例、研究简介、第 0 阶段计划里"确认文献物种的导入许可"一项。

| 字段 | 内容 |
|---|---|
| 发出 | 第 _ 周；发出的版本：v0.2 |
| 跟进 | 第 _ 周 / 未跟进 |
| 答复 | 第 _ 周 / 无回复 |
| (a) 导入 | 同意 / 部分同意（范围：___）/ 不同意 / 无回复 |
| (b) 许可 | CC0 / CC BY 4.0 / MIT（保留声明）/ 未明确（待确认） |
| (c) 署名与名称 | 署名格式：___；原名和分类：原样保留 / 其他：___ |
| (d) 参与 | 愿意提建议或审阅 / 暂不 / 未提及 |
| 附带条件 | ___ |
| 采取的做法 | 见下文"可能的回复"表的对应行 |

### 8. 变更记录

| 日期 | 版本 | 变更 |
|---|---|---|
| 2026-10-20 | 草稿 v0.2 | 按对抗式审查修订（见文首） |
| 2026-10-23 | 草稿 v0.2（内部说明修订，邮件正文未改） | 跨文档一致性修订：第 3 节加决策日志 E.3"随机汤包不包括文献邻域"的发信前检查；第 5 节注明文献种默认不出图版（PG13）；第 6 节第 5 条加 E.3"没有许可时教程去哪里"的标记（线框 UX7 更严）；"可能的回复"表注明预览起算点的建议和图版样张 |
| 2026-10-30 | 草稿 v0.2（内部说明修订，邮件正文未改） | 按 2026-10-30 的拍板更新：第 2 节按 D-018 执行；第 3 节的随机汤一项按 D-049 勾掉；第 6 节第 2 条按 D-042(a)，第 5 条注明被 D-050 取代；D-040 的标记保留 |

---

## Email

**Subject:** Permission request: Lenia species in an open catalog (Naturalis, working title)

Dear Bert Chan,

My name is [Founder name]. We have not been in touch before. I am writing to ask for your permission and advice before using species from your Lenia work in a project I am designing.

Naturalis (working title) is a planned browser game set in Lenia, in which players act as naturalists: they find, describe and name lifeforms, and together build a permanent, open catalog. It is at the design stage only: one person (me), with no code, team, funding or players yet, and the name may change. A brief with a one-page summary is here: [research brief link].

With your agreement, and only then, I would like to include the species described in "Lenia: Biology of Artificial Life" (*Complex Systems*, 2019) as literature records under their original names, credited to you. A player who re-finds Orbium would then see it as your species, not register it as new. I would like to ask:

(a) May we import the parameters and patterns of the species in your 2019 paper, taken from the Lenia repository, into an open catalog? Your later work would be a separate question.

(b) I propose releasing catalog data under CC0, which lets anyone, companies included, reuse it for any purpose without giving credit; credit would rest on the records and on citation norms. For data from your work, this would mean you dedicating it to the public domain under CC0, or, if you prefer, licensing it to everyone under CC BY 4.0, which requires attribution. As I understand it, neither can be revoked once data is released, so I am asking first. Which would you prefer?

(c) How would you like to be credited, including the year (2018 on arXiv, or 2019 in the journal)? Records would show your names and classification as published. Later community revisions (for example, if two species turn out to be connected) would be recorded with their history, and your original classification would stay visible. Is that acceptable? We would not place new species in your genera, or move your species to other genera, unless you are happy with that. Our draft tutorial retraces your discovery of Orbium. Its working title does not use your name; we would name it after you only if you would like that.

(d) If you are ever interested, your advice or review would be very welcome, at any stage and with no obligation.

A few things you should know:

- The catalog would use a fixed integer version of Lenia (lenia-q16), meant to reproduce bit for bit on any machine; our first four weeks of work test whether it does. Rounding to it may change how some species behave. We would label those clearly rather than adjust them to fit.
- For internal testing only, we plan to run about ten of your species, such as Orbium, from the parameters published in your paper, plus simple variants of them (for example a larger radius or a finer time step, and random soups under those parameters). Nothing about your species will be published without your agreement, and we will stop if you prefer.
- The project has no income. If it goes ahead, a small company or a non-profit (not yet decided) may run it, funded by optional extras such as cosmetics and prints, and by grants; the catalog data would stay free. We will never sell naming rights or priority, or issue tokens or NFTs, though under CC0 we could not control what others do.
- Your code is MIT-licensed, and we will follow its terms wherever we use it. I am not sure whether that licence is meant to cover the species data, so I am asking rather than assuming.
- A "no" is completely fine. We would then copy none of your data into the catalog. We would only cite your paper and, when a player re-finds one of your species, point to your paper rather than treat it as new.

If you agree to any part of this, I will send you a preview of how your species would appear before they are shown to anyone, including invited testers.

Thank you for Lenia, and for reading this.

With best wishes,
[Founder name]
[email] · [website]

---

## 可能的回复与对应做法（内部，发送时删除）

| 可能的回复 | 我们怎么做 | 记录与同步修改 |
|---|---|---|
| 全部同意，CC0 可以 | 回信致谢并附第 5 节的确认句；按 (c) 的要求定署名格式；预览至少在内测开放前 3 周（暂定，D-040 第 2 条；PRD 第 5 节建议改为从测试者第一次看到文献种往前算，见决策日志 E.4）发出，同时附图版样张（纸面生成器 PG13），没有回应就提醒一次，然后按已获许可的范围执行并记入决策日志；第一阶段按 CC0 导入 | 决策日志；§14.5 `license`；§8.5(9) 署名示例 |
| 同意导入，但希望用 CC BY | 按第 2 节事先定好的选项执行（暂定选项 A′：文献条目单独用 CC BY 4.0）；回信确认署名文字 | 决策日志；§2.2、§14.9 加"文献导入部分除外"；导出按条目标许可 |
| 同意导入，但要求数据按 MIT 处理 | 按第 2 节选项 A″：导入条目的 `license` 标 MIT，附 Chan 的版权声明和 MIT 全文，导出时一并附上；回信确认 | 决策日志（D-018）；§14.5 `license`；MIT 用在数据上是否合适，列入法律备忘 |
| 同意导入，但没提许可，或只说"代码是 MIT，随便用" | 视为同意 (a)；回一封短信，只确认 (b)；确认之前不公开发布导入的数据 | 决策日志标"(b) 待确认" |
| 有条件的同意（只导入部分物种、改名、要求先审阅等） | 照条件做；做不到的条件如实说明，不默认接受 | 决策日志逐条记下条件 |
| 同意引用，但不同意再发布数据 | 致谢；执行第 6 节的退路。内部使用以对方的答复为准（邮件已说明内部测试），不再另提请求 | 决策日志；GDD §19.1、第一阶段 PRD 把"文献导入"改为"文献引用表" |
| 明确拒绝 | 回信致谢，不再提任何请求；执行退路；对外不说"作者拒绝了" | 同上 |
| 部分物种或数据另有作者或权利人 | 向相应的人另外请求；没有确认的部分按退路处理 | 决策日志按物种记录许可状态 |
| 愿意提建议或审阅 | 致谢；问对方方便的方式和频率；再次写明没有任何义务。对方书面（邮件即可）同意担任顾问，并确认研究简介里的条件（每月约 1 小时，暂定；只提建议，决定由创始人做；目前没有报酬；随时可以退出；公开列名须本人同意；即 D-040 第 7 条）之后，才计入 G3 的"≥ 2 位顾问" | 决策日志；研究者联系名单 |
| 对项目本身有顾虑（命名权归玩家、游戏化、整数化等） | 认真回复，不辩解；说明可以调整的地方（例如未复现的物种只列为文献引用，不导入）；把意见带回决策日志讨论，不当场承诺修改设计 | 决策日志；必要时修订 GDD |
| 先提问题，或想先看简介、预览 | 尽快如实回答，项目状态照实说；预览要等第 0 阶段测试之后才有，时间如实告知 | 决策日志记下问题和回答 |
| 第 4 周末仍无回复（已跟进一次） | 不视为同意；第一阶段按退路规划；不再就此事追问 | 私密记录记"无回复"；决策日志按 D-042(e) |
