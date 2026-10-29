# 数据模型、事件日志与导出格式

> 版本：v0 底稿 · 日期：2026-10-21 · 状态：待创始人审阅 · 负责：后端与数据工程师（到岗前由创始人暂代，D-044）；导出部分请顾问研究者一起审阅 · 《博物志》和 Naturalis 都是暂定名（D-013）
>
> 依据：[GDD v0.2](../naturalis-gdd.md) §8.2–§8.7、§10.2、§11.2、§14.1、§14.4、§14.5、§14.9、§19.1；[决策日志](../decision-log.md)；[命名法规 v0](../naming-code-v0.md)（下称"法规"）；[宇宙规范草案](../spec/lenia-q16-draft.md)（下称"规范"）§3、§4、§8；[法律备忘](../legal-memo.md) §3、§7；[第一阶段 PRD](../product/phase1-prd.md)（同为 ★ 草稿，功能范围以它为准）。截止和拍板人只以决策日志为准。
>
> **标记**：【待 D-0xx】＝依赖提议中的条目，拍板前不是决定；【新提议 Nn】＝本文新提的选择，日志里还没有条目，汇总在文末"需要新开的决策条目"；暂定＝猜测的数字；[verify]＝待核实的事实；[待法务确认]＝法律问题，本文不下结论。只有 A 组（D-001–D-012、D-041）已采纳。
>
> **这是底稿**：README 把本文列为第一阶段开工前必须完成的三份文档之一。第 0 阶段还没执行。访谈计划 §7.3(4) 要求影响数据模型的字段请求在本文动笔前定；本稿先于访谈起草，做不到，所以 W4 第 3 天冻结的高优先级字段请求要在本文**定稿前**逐条处理（已覆盖、加字段或"再问"）。assay v1、dedup v1 和采集交接规格写出后也按它们修订。整个优先权模型以 D-015 选 A（第一个有效描述者）为前提【待 D-015】；选 B 时 §4.2、§5 重写。

---

## 1. 范围与原则

**本文定**：实体和字段（§3）；标本、描述和条目的状态机（§4）；承诺的构造（§5）；只追加的事件日志（§6）；内容寻址和分享链接（§7）；每周导出和 DOI 快照（§8）；编号、版本与迁移（§2.2、§9）。

**不在本文**（链接是 [PRD](../product/phase1-prd.md) §12 的计划路径，文档还没写）：状态、参数、核、查找表和轨迹哈希链的字节格式（规范 §8，本文只引用）；鉴定测量的定义（[assay v1](./assay-v1.md)）；同种判定（[dedup v1](./dedup-v1.md)）；观察记录和起点引用的判定口径（[采集交接规格](./collection-handoff.md)）；保留期的法律依据（[★ 隐私政策、用户协议与同意流程](../data/privacy-terms-consent.md)）；存储选型、密钥管理和接口（[系统架构与接口契约](./architecture.md)）；第一阶段做哪些功能（PRD 第 3、4 节；本文给推迟的功能只预留格式）。

**原则**：

1. **日志是事实的来源。** 条目、物种页、周报和导出都是由事件日志和内容寻址对象算出来的投影（GDD §14.1）。投影可以重算，日志不改。
2. **发布了就不改。** 事件、承诺、证书、查重报告、每日链头和导出包一经发布永不修改。更正一律追加新事件或新对象（法规第 5 条第 6 款）。
3. **登记之前默认私密。** 没有登记的东西只公开加了随机数的承诺哈希（法规第 9 条）。例外只有三种：普查标本进入待描述库时公开（D-008）；玩家主动分享的标本，凭链接可见（§7）；评审确认为分布区标本、采集者同意公开的（§3.3 参考标本，dedup v1 第 7.5 节）【待 DD10】。
4. **信封里没有个人数据。** 链只覆盖信封（§6.1），信封里只有伪名和哈希。载荷里直接写出的名字只有登记所用名称字段里的命名者署名（法规第 27、31 条），需要时可以在公开视图里隐去（§6.1）。评审和裁决的参与人在载荷里只写署名标识，按本人同意经署名表显示（法规第 50、54 条）【待 PC18】；其他显示名同样只在链外的署名表里，删除账户时可以处理【待 D-048】。登记后公开的探索路径、观察时间和描述文字经署名表与显示名相连，也可能算个人数据 [待法务确认，法律备忘 Q24(d)]。
5. **数值精确。** 状态和参数只用规范 §8 的二进制记录；被哈希的对象里不出现 JSON 浮点数。
6. **永久标识不含暂定名。** 编号、格式标签和文件名都不用《博物志》或 Naturalis（D-013）【新提议 N2】。

**只有创始人时**（D-044）：日志、承诺和每周导出属于 GDD §19.1 "不能砍的"底线，格式不降级。功能的砍单（如野外笔记只导出图片）按 PRD 第 7、8 节，本文不另定。实现按下表降级。

| 部分 | 有团队时（3–5 人，GDD §19.2） | 只有创始人时 |
|---|---|---|
| 存储 | 一个关系库加对象存储；投影由脚本重算 | 同左；日志是一张只追加的表，库里禁止更新和删除 |
| 每日链头 | 公开仓库加第三方时间戳服务 | 只由定时任务推到公开仓库 |
| 导出与研究者工具 | JSONL 加 CSV 摘要，Parquet 按研究者需要再加；读取、核对清单与链、核对承诺、逐条复跑 | 只有 JSONL 和条目 CSV；复跑按 PRD F36 抽样 |
| 删除账户时的私密载荷 | 按账户加密，删除时销毁密钥（§6.4） | 直接删除私密库里的行；备份按轮换期过期，期限见隐私文档 |

## 2. 通用约定

### 2.1 编码

**规范化 JSON（NTJ）**【新提议 N1】。所有被哈希或签名的对象（承诺、事件信封和载荷、证书、查重报告、探索路径、轨迹分段列表、每日链头、导出清单）先按 NTJ 序列化成字节，再取 SHA-256（【待 D-021】）。NTJ 是 RFC 8785（JCS）加下面的限制：

- 键只用 ASCII，按字典序；没有空白；UTF-8。文字在入库时做 Unicode NFC 规范化、换行统一为 LF，存储和公开的就是规范化后的文字，核对时不再变换。
- 数字只用整数，绝对值 ≤ 2^53 − 1，更大的写成十进制字符串。浮点量（描述性测量，分层【待 D-034】）写成十进制字符串，写法同 ECMAScript 的 `Number.prototype.toString`（也就是 JCS 写数字的方法），如 `"0.71"`、`"0.00001"`；Python 的 `repr` 在指数形式上不同（`1e-05`），要用专门的函数 [verify：用测试向量核对]。不允许 NaN 和无穷。
- 可选字段缺省时省略，不写 null。表示集合的数组按升序排列：字符串按 UTF-8 字节，对象按各自的 NTJ 字节；表示序列的数组保持原顺序。时刻写成 `YYYY-MM-DDTHH:MM:SS.sssZ`（UTC，固定 3 位毫秒），日期写成 `YYYY-MM-DD`。
- 哈希写成 `sha256:` 加 64 个小写十六进制字符（规范 §8.1）；随机数 128 位（暂定，法规第 9 条），写成 32 个小写十六进制字符；其他二进制写成不带补位的 base64url。
- 每个顶层对象都带 `fmt`，形如"名称/主版本"（如 `collection-commitment/1`）。名称不同就是不同的哈希域，一类对象的哈希不会和另一类相同。事件载荷不带 `fmt`，它的类型和版本由信封的 `type`、`tv` 给出（§6.1）。

说明：为什么不像规范 §8 那样用定长二进制。承诺和事件里有变长文字，字段会随版本增加。加上这些限制后，JCS 对每个对象只有一种合法编码，D-026 担心的"两种合法编码"不会出现。Python 的 `json.dumps(obj, sort_keys=True, separators=(",", ":"), ensure_ascii=False)` 在这些限制下与 JCS 输出相同 [verify：用测试向量核对]。规范 §8 的二进制记录不变，在 NTJ 里只以哈希出现。

### 2.2 编号

| 编号 | 格式 | 分配 |
|---|---|---|
| 永久编号 | `LEN-` 加 6 位十进制，从 `LEN-000001` 起；用完 6 位后加位，如 `LEN-1000000`（法规第 3 条，暂定）。按数值排序，不按字符串 | 登记生效或建立文献条目时，由日志的单一写入者在写该事件的同一事务里取下一个号：不并发发号，没有空号，不设保留号段，号码不含任何含义【新提议 N3】。异名和撤回条目的号永不复用（GDD §8.5(6)）。`literature_reference` 也占号【待 D-042(a)】 |
| 临时编号 | `prov:YYYY-MM-XXXXXX`：采集时服务器的 UTC 年月，加 6 位大写十六进制随机数（GDD §8.5(7)） | 随机抽取，与已发出的全部临时编号比对，重复就重抽；某月发出超过 2^22 个时，当月余下的改用 8 位（暂定）。解析器接受 6–12 位。已发出的号永不复用，包括被玩家删掉的标本【新提议 N3】 |
| 事件序号 | `ev:` 加十进制序号，从 1 起 | 写日志时顺序分配（法规第 10 条第 3 款）。周报和史官引用事实时用它（GDD §8.8、§10.3），只能引用载荷已公开的事件 |
| 其他 | 普查 `CEN-0001`；新手区 `NZ-001`；悬赏 `BTY-0001`；分类评审 `RVW-2026-0001`、异议 `OBJ-…`、裁决 `RUL-…`、申诉 `APL-…`、分类修订 `REV-…`（按受理时的 UTC 年份编号）；观察记录 `obs:`、野外笔记页 `nb:`、推荐 `rec:`（各加 12 位小写十六进制随机数；`rec:` 私密，只在推荐缓存和好奇心事件里【待 AI8】）；周报 `WR-2026-W46`（ISO 周）【待 AI14】；证书、悬赏判定结果、查重报告、探索路径用内容哈希：`cert:sha256:…`、`judge:sha256:…`【待 SB14】、`dedup:sha256:…`、`trail:sha256:…`。证书和判定结果的编号取去掉 `sig` 之后的哈希，密钥轮换后重签编号不变，同一编号可以有几份签名【待 AS14】【新提议 N3】 | 位数用完时加位，规则同 LEN |
| 保留 | `EXP-`（实验）、`CON-`（构造），6 位，第二阶段启用（GDD §9.1、§9.2；法规第 2 条第 3 款）。其他宇宙家族和变体规范的前缀，开放登记前由法规修订写明（法规第 2 条第 2 款）。地名的编号随地名功能另定（法规第 28 条） | — |
| 测试沙盒 | 团队的测试登记用 `TST-`，放在独立的库里，不占 LEN 编号和学名，不进日志的公开视图，不进导出（法规第 49 条第 2 款）。alpha 期的登记就在 LEN 里，没有单独的命名空间【待 D-030】 | — |

### 2.3 字段标记与许可

| 标记 | 含义 |
|---|---|
| 无标记 | 公开数据，CC0 1.0【待 D-018；选 B 时为 CC BY 4.0】 |
| ‡ | 人写的自由文字（玩家、评审和研究者写的），CC BY 4.0，署名见署名表（§3.1）【新提议 N14，细化 D-018】。团队写的说明文字（README 等）同样用 CC BY 4.0【新提议 N14】 |
| 私 | 私密：不进日志的公开视图，不导出 |
| 延 | 暂缓：登记时（普查标本在进入待描述库时）才公开；此前只公开承诺哈希 |
| 文 | 文献导入的数据：选 A 时 CC0；A′ 时 CC BY 4.0；A″ 时 MIT，附 Chan 的版权声明（D-018，法律备忘 §3.4 第 4 点） |

图版由生成器从数据重算，不是存储的字段，条目投影只记它的哈希（§8.3 `plate`【待 PG8】）；标什么许可、谁是许可人，按法律备忘 Q16 [待法务确认]。"永久"指只合并、不删除（GDD §8.5(6)）。凡涉及个人数据的保留期，都以隐私文档为准 [待法务确认]。

## 3. 实体与字段

### 3.1 账户、署名与同意

| 实体 | 字段（标记见 §2.3） | 公开 | 保留 |
|---|---|---|---|
| 账户 | `account_id` u64 私 · `email` 私 · `auth` 私 · `created_at` 私 · `adult_declared` 私（自我声明加邀请制）与 `region` 国家码 私【待 D-031】· `invite` 私：邀请码和标签"批次-渠道类型-语言"（如 `B1-C4-en`，只用于内部切片）【待 RC6】· `player_id` · `display_name`（署名表用）· `authority_form` 拉丁字母署名形式（法规第 27 条第 2 款）· `salt` 32 字节 私 · `roles` team / panel / researcher · 配额、处罚 私 | 玩家 ID、显示名；角色中的评审组成员和研究者按本人同意公开【待 D-040 第 7 条】 | 账户存续；删除见 §6.4 |
| 署名表 | `object`（LEN、prov、RUL-… 等）· `role` collector / describer / keen_eye / objector / reviewer / ruling_participant【待 PC18】/ census_selector / bounty_publisher（第二阶段）/ bounty_codesigner / nominator【待 SB14】/ solver · `display` 显示名或"匿名贡献者" | 公开；**不进链**，每周重新生成 | 删除账户后，以后的导出改为"匿名贡献者"【待 D-048】 |
| 署名标识 | `sha256:` 串，构造见 §5 | 公开 | 永久；账户盐销毁后不能再关联到人 |
| 同意记录 | `account_id` 私 · `level` 0 / 1 / 2 · `text_version` 同意文本的哈希 · `lang` · `set_at` · `source` 注册或设置页 · `ethics_ref`（只在 2 级；审批或豁免之前不提供 2 级，GDD §19.1、D-036） | 全部私密，只追加 | 账户存续；删除后 3 年，只留这几个字段（暂定，隐私文档 PC6）[待法务确认] |
| 条款接受 | `account_id` 私 · `doc` terms / conduct / tester_notes（用户协议、行为准则、测试者须知，法规第 52 条）· `text_version` 文本哈希 · `lang` · `accepted_at`【待 PC7】 | 全部私密，只追加 | 同同意记录 |

### 3.2 采集、鉴定与查重

| 实体 | 字段 | 公开 | 保留 |
|---|---|---|---|
| 采集记录 | `prov` · `spec` · `params` hP（参数记录本身 延）· `n` · `state` S（状态记录 延）· `trail` · `method` manual / tool / census（法规第 19 条）· `link_source`（经分享链接得到时，法规第 11 条第 3 款）· `precheck` ok / phantom【待 D-029】· `precheck_notes` split / expanding / morphed / unstable（只是提示）· `precheck_by` server / client（服务器复跑为准，client 只用于客户端报的幻影）【待 CH8】· `commitment` · `nonce` 延 · `seq`、`ts` · `near_literature` 私（法规第 45 条）· `pool`（在待描述库） | 承诺哈希立即公开；其余 延 | 公开了的：永久。玩家采集箱里没公开的：账户存续，玩家可删，日志里的承诺哈希保留。鉴定没通过的：状态记录和证书保留 12 个月（暂定）或到账户删除，以先到者为准，供 D-027 重新鉴定时使用【新提议 N11】 |
| 探索路径 | `fmt` trail/1 · `seed`：播种方式 `kind`（soup 汤、brush 画笔、catalog 图鉴拖入、link 链接、import 导入【待 CH18】；gen 生成器保留，PRD 第 4 节推迟）与初始状态哈希 · `steps`：按**模拟步数**记的参数改动和操作，**不记墙钟时间**【新提议 N9】· `step_unit`：步数一律按 1,000 步取整，各级同意相同（步数会透露停留多久）【待 D-039、PC2】· `from`：拖入或"从这里出发"时，起点段对象 `origin-segment/1`（起点 LEN、状态和参数哈希、之后的操作只记顺序）的哈希 · 其余（`start`、`capture`）见采集交接规格第 9 节【待 CH10】 | 延 | 同采集记录。起点段对象单独存放：只为登记成新种的保留，其余可以删掉，承诺照样能核对（交接规格第 13 节）【待 D-039、CH15】 |
| 鉴定证书 | `fmt` assay-certificate/1 · `spec`、`params`、`n`、`state`、`kernel` hK、`growth_lut` hG · `engine` 版本与构建哈希 · `suite` assay 版本 · `verdict` pass / fail · `dims_tested` 测了哪些离散化维度（D-003 待议部分）· `survival` {time_units, steps, survived, died_at_tu}【待 D-033】· `locality`、`locality_recheck` · `classes` · `trajectory` {interval, steps, chain, segments} · `measures`（十进制串，分层【待 D-034】；没测的项写 `"untested"`，PRD F16；不适用的省略）· `discretization_runs` [{R, T, n, steps, status, survived, locality, classes, chain, segments}] · `platonic`（判不了时省略）· `prng_seed`（自愈测试；没做时省略）· `assay_frame` 鉴定帧（基准运行最后一步的状态哈希，查重从它开始）· `birth_reproducible`（PRD 第 4 节推迟，第一阶段省略）· `issued_at` · `sig`。补充字段的定义见 assay v1 §6.1【待 AS14】 | 延（随所鉴定的标本） | 永久。主体永不修改；套件 bug 和补测都签发新证书，写 `certificate.corrected`（原因码 `bug_fix` / `supplement`），前者旧证书标"已更正"，后者物种页写"已补测"【待 D-027、AS16】。观察挑战用的第 257–511 步状态哈希在鉴定时私存，不进证书（交接规格第 12 节） |
| 轨迹分段列表 | `{"fmt": "trajectory-segments/1", "interval": 256, "points": [[k, S_k], …]}`，k 从 0 起到最后一步 L，与规范 §8.5 的链点一一对应；证书的 `segments` 是它的哈希（规范 §8.5 留给本文）【新提议 N1】 | 延 | 永久 |
| 查重报告 | `subject` prov、LEN 或一对条目 · `purpose` assay / description / registration / objection / audit / recheck · `as_of_seq` · `fingerprint` · `frames`（8 个规范帧状态哈希拼接后的哈希）· `model` {name, version, code}；v1 没有学到的权重，省略 `weights`【待 D-034】· `nearest` [{ref, distance, mirror, rotation, phase}] · `continuity` [{to, passed, chain, direction, result, moves, break_at, spent}] · `decision` auto_new / auto_same:LEN / grey（报告出具时还没有评审编号，开评审时由 RVW 引用报告）· `pool_match`（同种的待描述库标本）· `reason` literature_staged / contest · `thresholds_version`。定义见 dedup v1【待 DD16】 | 延；`nearest` 和 `auto_same` 里指向私密对象（审核中的描述、文献暂存）的项，公开视图写 "private" | 永久。查重是时点报告：鉴定后、提交描述时、登记前各跑一次（法规第 10 条第 1 款）【新提议 N21】；异议、抽检和升版后的重查按需另出（`purpose`）。按需的连续性检验是单独的内容寻址对象，由 RVW、异议的 `evidence` 引用 |

### 3.3 描述、名称与条目

| 实体 | 字段 | 公开 | 保留 |
|---|---|---|---|
| 描述 | `desc`（第一个描述提交承诺的哈希）· `prov`、`state` · `diagnosis` ‡ · `behaviour` ‡ · `observations` · `ai` 分项声明 [{part, help, tool}]（法规第 17 条第 3 款），`tool` 写"名称/版本"，如 `description-assistant/1.0.0`【待 AI3】· `name` 名称子承诺 · `describer` 署名标识 · `nonce` · `commitments` 改名后的承诺链 · `status`（§4.2）· `quota_week` 私 · `near_literature` 私 · `eponym_pending` 私 | 延：登记时公开全文和承诺原文（法规第 31 条第 3 款）；没有成立的只公开承诺哈希（法规第 9 条） | 登记了的：永久。"搁置"后失效的：账户存续（法规第 38 条可能按原提交时间恢复）。其他没有成立的：内容 12 个月后删除（暂定），哈希留在日志【新提议 N11】 |
| 名称字段 | 见 §5：`genus`、`epithet`、`new_genus.note` ‡、`common`、`etymology` ‡、`authority`、`nonce`【待 D-017】 | 延：只公开登记所用的一版；被退回的只公开子承诺哈希（法规第 30 条） | 同描述 |
| 条目 | 见 §8.3 | 登记时公开。`under_review` 只存在于私密视图：名字暂时保留（法规第 4 条），内容在登记前不公开（第 9 条、第 10 条第 1 款） | 永久 |
| 模式标本 | `spec`、`params`（参数记录及解码后的整数）、`n`、`state`、`kernel`、`growth_lut`、`trajectory`、`cert`、`trail` | 公开 | 永久，登记后不可修改（GDD §8.5(2)） |
| 参考标本 | `ref` 状态哈希 · `entry` LEN · `origin` literature_rediscovery / former_type:LEN（法规第 36 条第 2 款）/ range_confirmed（分布区标本：评审判同、规则点不同，采集者同意公开快照和署名；确认它的 RVW 随之公开，dedup v1 第 7.5 节）【待 DD10、DD16】· `spec`、`params`、`n`、`state`、`cert` · 采集者见署名表 | 公开；挂在 `literature_reference` 下的不写与原文参数的差别（法规第 44 条第 3 款） | 永久 |
| 文献记录 | `entry` LEN · `status` · `original_binomial`、`original_author`、`original_classification`（始终可见【待 D-040 第 3 条】）· `year`【待 D-042(c)】· `source` DOI 或 arXiv 编号 · `params` 文（只在 literature、literature_unreproduced）· `unreproduced_reason` · `license` · `permission_ref` 私 · `preview_done` 私 | `literature_reference` 只有学名、原作者署名和原文链接（法规第 42 条）。有许可时，导入先在私密暂存区完成，到 PRD §2.3 的 G-文献可见（预览满 3 周，暂定）才写 `literature.imported` 事件、分配 LEN、进导出【待 D-040 第 2 条】【新提议 N18】 | 永久 |

### 3.4 评审、异议与裁决

| 实体 | 字段 | 公开 | 保留 |
|---|---|---|---|
| 分类评审 | `RVW-…` · `case` grey / new_genus / recombination / split / merge · `reason`（grey 的 `contest`：对自动判同的复核申请；`literature_staged`）【待 DD11】· `items` · `evidence`（查重报告、连续性检验对象、帧序列对象的内容哈希；不存视频，dedup v1 第 8.3 节、系统架构）· `votes` [{reviewer 署名标识, vote same / different / unsure, reason ‡}] · `outcome` | 结论公开；逐票公开，评审人按本人同意经署名表显示（角色 reviewer），不同意的写"一位评审人"（法规第 50 条第 2 款）【新提议 N17】【待 PC18】；涉及未登记描述的，随登记公开 | 永久 |
| 异议 | `OBJ-…` · `target` LEN，或悬赏判定结果 `judge:…`【待 SB14】· `ground` a / b / c / d（法规第 32 条第 3 款）；对判定结果只有 bug / eligibility / priority 三种（种子悬赏第 7.2 节）· `statement` ‡ · `evidence` · `objector`（署名表；邮件提出的不署名）· `in_period` · `status` | 公开 | 永久 |
| 裁决 | `RUL-…` · `cases`（异议、违规、申诉）· `evidence` · `articles` 适用的条款 · `conclusion` · `reasons` ‡ · `participants`（署名标识，按同意经署名表显示，角色 ruling_participant【待 PC18】）· `independent`（法规第 55 条第 2 款）· `cure_until`（补正期，法规第 34 条） | 公开；同意书、联系方式不公开（法规第 54 条）；账户层面的措施不公开 | 永久 |
| 申诉 | `APL-…` · `ruling` · `statement` ‡ · `appellant`（署名表）· `outcome` | 公开 | 永久 |
| 属与分类修订 | 属：`genus` · `type_species` LEN · `proposed_in` 描述承诺 · `priority`（序号或文献日期）· `status` proposed / accepted / rejected / merged_into · `literature_genus`【待 D-040 第 9 条】。修订：`REV-…` · `kind` recombination / merge / family · `before`、`after` · 依据的评审或裁决 | 公开；新属在所属描述登记前 延 | 永久，每次修订留痕（GDD §8.4） |

### 3.5 个人记录与考察

| 实体 | 字段 | 公开 | 保留 |
|---|---|---|---|
| 观察记录 | `obs:…` · `target` prov 或 LEN · `observer` 署名标识 · `at`（服务器凭据的签发时间）· `mode` exact / float · `check` challenge / none（exact 用挑战校验）【待 CH14】· `duration_s`（法规第 16 条） | 延：记录存在浏览器，随描述上传（CH14）；随描述登记公开，时间粗化到日，时长取整到分钟【新提议 N10】 | 同描述 |
| 观察志条目 | `account_id` 私 · `entry` LEN · `first_seen` 日期 · `via` collected / exact | 私密；个人档案只显示计数，玩家可选公开；导出只给每个条目的观察人数，不足 5 写 "<5"【新提议 N10】 | 账户存续 |
| 慧眼标记 | `prov`（待描述库）· `flagger` 署名标识 · `seq` | 记功时公开（署名表）；0 级也保留，只用于记功【待 D-039】 | 记了功的永久；其余随账户 |
| 野外笔记页 | `nb:…` · `account_id` 私 · `date` · `items`：服务器已有记录的引用（临时编号、LEN、证书）、观察志新增、批注（平台不公开，不标 ‡；玩家导出分享时的许可【待 PC16】）。路线、起点种、"有趣"标记和观察摘要任何级别都只留在浏览器，不上传【待 PG15、D-039】· `hidden_params` 导出中恒为 true，第一阶段没有关掉的开关（D-015 建议）【待 PG16】 | 私密；客户端生成，只在玩家按"保存"时上传；"分享"就是导出文件，第一阶段不设托管的分享链接；不进日志，不进导出【新提议 N20】【待 PG16】 | 账户存续，玩家可删 |
| 测绘 | 个人：`account_id` 私 · `slice` · 去过哪些格子的无序位图（不含时间和顺序）· 测绘面积和面积第一次 > 0 的日期【待 TM13】。全局：`slice` · `cell` · `terrain` dead / chaotic / life_band · `proto`（如 `terrain/1.0.0`）· `runs`（4 个汤的结果字母）· `check`（4 个末态哈希拼接后的哈希）· `sources` {census, registered, reports}【待 CA4、CA6】 | 个人私密；测绘面积默认只给本人看，本人可以选择公开（同观察志）【待 CA7】。全局公开，只含匿名计数，`reports` 不足 5 批不公开；服务器只在地形区里算地形（普查文档第 2.5 节） | 个人随账户；全局永久【新提议 N10，待 D-039 扩展】。个人位图按隐私文档 PC3【待 D-039】：0 级只在浏览器，服务器只收面积和日期；1 级以上存私库，只给本人看 |

### 3.6 普查、新手区、悬赏、引用、周报与行为类别

| 实体 | 字段 | 公开 | 保留 |
|---|---|---|---|
| 普查运行 | `CEN-…` · `spec` · `region`（格点上的参数范围）· `algorithm` {name, version, code 哈希} · `prng_seed`：`census.started` 只写取种规则 {rule: census-seed/1, beacon_after_seq}（种子取之后发布的第一个每日链头），`census.ended` 写数值【待 CA9】· `budget` 核分钟，计划和实际，不写金额【待 CA11】· `chosen_by` 团队；顾问只在本人同意时列名【待 D-040 第 7 条】· `reasons` ‡ · `started_at`、`ended_at` · `outputs` {specimens, cells} · `initiator`、`priority_until`（第一阶段为空，D-008；第二阶段【待 D-016】） | 公开 | 永久 |
| 新手考察区 | `NZ-…` · `spec` · `region` · `active_from`、`active_to`（事先公布；轮换时实际结束日由替代区的 `replaces` 和 `active_from` 决定，已发布的事件不改）· `backup`：备用区的区域和理由、唯一可能的轮换日期和触发规则 · `replaces`：只在轮换时写被替代的 NZ 编号【待 CA15】· `reasons` ‡ | alpha 开放前公开【待 D-043】 | 永久 |
| 悬赏 | `BTY-…` · `fmt` bounty/1 · `question` ‡ · `source`（Q 编号）· `publisher`：第一阶段固定为 team【待 SB11】· `co_designers`（研究者，按同意署名，不同意的写人数）· `scope`、`eligibility`、`mode` discovery / collection / record、`record_metric`、`tracks` · `judge` {id, version, code 哈希, framework, spec} · `baseline` · `preregistration` ‡、`credit_commitment` ‡ · `reward` 只有署名和荣誉【待 D-019】· `announced_at`、`opens_at`、`deadline` 可空、`objection_days` · `publication_policy` ‡ · `data_license` · `status` announced / open / closed 加 `closed_reason`。定义见种子悬赏第 8 节【待 SB14】 | 公开 | 永久 |
| 判定结果 | `judge:sha256:…` · `fmt` judge-result/1 · `bounty` · `submission`：第一阶段只判 LEN，不收未登记的标本【待 SB4】· `submitter` 署名标识（只有人工提交的；登记时自动判定的没有）· `nominator` 可空 · `track` human / tool_census · `priority` 描述提交事件的序号 · `cert` · `judge` 版本 · `engine` · `result` pass / fail / ineligible / not_testable 加原因码 · `metrics` · `run` 链哈希 · `supersedes`、`reason`（更正时）· `sig`。决定成败的量逐位可复现【待 D-034、SB14】 | 公开（只判已登记条目，不需要暂缓） | 永久 |
| 引用 | `kind`：第一阶段只记 origin（起点引用）；academic、construct、experiment 预留，第一阶段不记（PRD 第 4 节）· `citing`、`cited` LEN · `evidence` 起点段对象的哈希（§3.2）或 DOI · `self` · `verified_by` | 公开 | 永久 |
| 周报 | `WR-…` · 内容寻址的 `weekly-report/1` 对象：期号、截止序号（同每周导出）、事实表 `weekly-facts/1` 的哈希、每句的中英槽位文本与引用 `ev:…`、提示词和规则版本、模型标识；更正版另有 `supersedes`【待 AI14、AI15】 | 公开；只存槽位文本，显示名在渲染时取（AI 助手第 4.8 节） | 永久；更正发新版，旧版不改（隐私类错误的旧版网页不再显示） |
| 行为类别 | `class` 枚举串 · `label` {zh, en} · `definition` ‡ · `detector` 版本 · `defined_by`（机器类别写 assay v1；玩家提议的见署名表，第一阶段不开放，PRD 第 4 节）· `status` | 公开 | 永久 |
| 地名 | 推迟到 alpha 之后（GDD §19.1 注）。第一阶段不建表，只预留事件类型 `place_name.*`；编号和字段随地名功能另定（法规第 28 条） | — | — |

### 3.7 好奇心事件

| 实体 | 字段 | 公开 | 保留 |
|---|---|---|---|
| 好奇心事件 | `subject` 内部伪名 私 · `session` 私 · `kind` interesting / pairwise / why / dwell / skip / zoom / move（GDD §11.1；第一阶段只记 interesting、skip、dwell、zoom，PRD F31，事件字典见遥测与指标口径）· `params`（格点）· `target`（可以是推荐编号 `rec:`）· `value` 小整数档位（如停留、放大的档）· `client_time`（客户端上传前和入库时各截到 UTC 整点）【待 TM10】· `consent_level` 记录时的级别 | 私密。单独存放：不进事件日志，不进每周导出【待 D-035】。2 级的匿名版本另行发布，以伦理审查为前提（GDD §11.4、D-036）。0 级和未成年人不收集（GDD §11.2）。运营遥测（崩溃、性能、登录）同样单独存放【待 D-039】 | 原始数据 24 个月（暂定）；撤回同意后停止使用并删除，期限待隐私文档 [待法务确认] |

## 4. 状态机

### 4.1 标本（采集记录）

| 从 | 触发事件 | 到 | 说明 |
|---|---|---|---|
| — | `collection.committed` | 采集箱 | 分配临时编号，记采集承诺 |
| 采集箱 | `precheck.done` | 已预检 / 幻影 | 整数实现从成体快照复跑，以服务器复跑为准；幻影不能送检，不进观察志【待 D-029、CH8】 |
| 已预检 | `assay.requested`，然后 `assay.certified` / `assay.failed` | 鉴定中，然后已鉴定 / 未通过 | 排队；未通过的保留期见 §3.2；原因码见 §6.3 |
| 已鉴定 | `dedup.decided` | 待描述（auto_new；grey 也到这里，带"查重存疑"标记）/ 已知种（auto_same，只记观察，法规第 14 条第 9 款） | 灰色评审在有人提交描述时才开（§4.2，dedup v1 第 8.1 节）【待 DD11】 |
| 已知种 | `review.opened`（case grey，reason contest） | 评审中 | 采集者对自动判同申请一次复核，占一条描述配额（暂定）【待 DD11】 |
| 评审中 | `review.concluded` | 待描述 / 已知种 | 判为不同种的回到待描述，计入自动判同错误（dedup v1 第 8.5 节） |
| — | `census.pooled` | 待描述（在待描述库） | 普查流程已做过鉴定和查重（暂定）；对所有人同时开放（D-008） |
| 待描述 | `description.submitted` | 描述中 | 同一件标本可以有多人描述（法规第 10 条） |
| 描述中 | `registration.effective` | 模式标本 | 不可修改 |
| 采集箱至待描述（玩家自己的） | 玩家删除 | 已删除 | 承诺哈希留在日志；临时编号不复用；鉴定中被删的写 `assay.failed`（cancelled） |

### 4.2 描述（私密，登记时公开全部历史）

| 从 | 触发事件 | 到 | 依据 |
|---|---|---|---|
| — | `description.submitted` | 排队 | 记承诺和序号，扣配额（法规第 8、18 条） |
| 排队 | `description.paused` | 暂停 | 同一个种已有在先的描述在审核中，或提交时的查重为灰色（含 `literature_staged`）（第 10 条第 1 款） |
| 暂停 | `review.opened`（case grey） | 分类评审 | 灰色评审从暂停开，先于文献对照和名称审核（dedup v1 第 8.1 节）【待 DD11】；法规第 30 条把它写成名称审核的结果，法规 v1 要改 |
| 暂停 | `description.shelved` | **搁置** | 在先描述登记生效（第 10 条第 2 款） |
| 搁置 | `description.lapsed`（superseded） | 失效 | 在先条目度过异议期；退还配额（暂定） |
| 暂停、搁置 | `description.resumed` | 排队（原提交时间） | 在先描述没有成立；或在先条目被分拆、撤回，同种判定被推翻（第 10 条） |
| 失效（因在先描述成立） | `description.resumed`（描述者申请） | 排队（原提交时间） | 在先条目分拆，所描述的标本属于分出来的种（第 38 条，暂定） |
| 排队 | `literature_check.started`，然后 `literature_check.concluded` | 文献对照，然后名称审核 / 失效 | 参数落在已发表物种附近的候选先人工对照原文；认定是文献种的记为再发现，描述失效、只记采集者功劳和观察（第 45 条）【待 D-042(b)】 |
| 排队 | `name_review.started` | 名称审核 | 24 小时目标（暂定，D-004）；文献附近的候选先经过上一行 |
| 名称审核 | `name_review.returned` | 待改名 | 最多 2 次（第 30 条之一） |
| 待改名 | `description.name_resubmitted` | 名称审核 | 72 小时内（暂定）；新承诺与第一个相连，优先权取第一个（第 8 条第 3 款） |
| 名称审核 | `review.opened` | 分类评审 | 新属（第 23、30 条） |
| 分类评审 | `review.concluded` | 排队 / 名称审核 / 待改名 / 暂停 / 驳回 | 灰色判为不同种的，`description.resumed` 回排队（原提交时间），需要时先做文献对照；新属通过的回到名称审核，没通过的 72 小时内改选已有属（第 23 条第 4 款）；与审核中的在先描述同种的暂停（第 10 条）；与已有名字同种的驳回，只记观察（第 14 条第 9 款），符合条件的成为分布区标本（§3.3） |
| 名称审核 | `registration.effective` | 已登记 | 条目进入 `valid`（§4.3） |
| 名称审核 | `name_review.rejected` | 驳回 | 缺少第 14 条的要件；或违规处理（第 56 条之一第 3 款） |
| 任何未结状态 | `description.lapsed` | 失效 | 逾期（72 小时、以人命名 14 天、累计 30 天）或第 3 次退回（第 30 条之一）；或描述者删除账户【新提议 N7】 |
| 任何未结状态 | `description.retracted` | 本人撤回 | 第 10 条第 2 款 |

### 4.3 条目（公开状态）

状态值沿用法规第 4 条。JSON 里拆成 `status` 加 `synonym_of`，`synonym_of:LEN-000045` 作为显示形式保留【新提议 N16】。每次状态变化另写一条 `entry.status_changed`。

| 从 | 触发事件 | 到 | 依据 |
|---|---|---|---|
| — | `description.submitted` | under_review | 只在私密视图，只有临时编号；描述没有成立就不留条目（法规第 4 条） |
| under_review | `registration.effective` | valid（异议期中） | 分配 LEN；记 `registered_at`；`objection_period_ends` = 登记时刻加 30 天（暂定） |
| valid | `objection_period.ended` | valid（已度过异议期） | 记 `objection_cleared_at`（第 35 条；GDD §18.1 按此统计） |
| valid | `ruling.issued`：同物异名或优先权成立 | synonym_of | 第 34、36、37 条 |
| valid | `ruling.issued`：认定为文献种 | synonym_of（指向文献条目） | 第 46 条；没有许可时先建 `literature_reference`【待 D-042】 |
| valid | `ruling.issued`：描述不合格、可以补正 | valid，开补正期 | `cure.opened`，14 天（暂定）；`cure.completed` 后名称和优先权不变（第 34 条） |
| valid | `ruling.issued`：伪造、违规行为；或 `cure.expired` | withdrawn | 第 34、40 条，第 56 条之一 |
| valid | `certificate.corrected` 后不再满足登记条件，按撤回流程 `ruling.issued` | withdrawn | 证书更正本身不改状态（D-027(4)）【待 D-027】 |
| valid | `name.changed` / `taxonomy.recombined` / `entry.split` | valid | 违规改名（第 56 条，原名在公开视图隐去）；新组合（第 39 条）；分拆（第 38 条）。编号和优先权不变 |
| synonym_of、withdrawn | `appeal.ruled` 成立，或 `ruling.issued` 推翻原判 | 原状态 | 第 37 条第 4 款、第 55 条第 3 款 |
| — | `literature.imported` | literature / literature_unreproduced | 第 42 条；在 G-文献可见时才写（§3.3），此前的导入和预览都在私密暂存区【待 D-040 第 2 条】 |
| literature ↔ literature_unreproduced | `certificate.corrected` | 另一种 | 【新提议 N18】 |
| — | `literature_reference.created` | literature_reference | 评审确认对应原文某个种时（第 42、45 条） |
| literature_reference | `literature.imported` | literature / literature_unreproduced | 之后取得许可、退路升级为导入时，沿用同一个 LEN【新提议 N18，待 D-042】 |

### 4.4 窗口与暂停

各窗口从触发事件的 `ts` 起算，到期由系统写事件。只有描述者一方的期限（改名、补同意、改属、累计 30 天）扣除审核方造成的延误，窗口记下暂停的累计时长（法规第 30 条之一）；异议期、补正、申诉不扣除。

| 窗口 | 长度（都是暂定） | 到期 | 依据 |
|---|---|---|---|
| 退回改名；以人命名取得同意；描述者一方累计延误 | 72 小时，最多 2 次；14 天；30 天 | 失效；按退回改名处理；失效 | 法规第 30 条、第 30 条之一 |
| 异议期 | 30 天，从登记起 | `objection_period.ended` | 第 32 条 |
| 补正（更正窗口）；申诉；违规改名 | 各 14 天 | `cure.expired` 后撤回；—；委员会选定 | 第 34、55、56 条 |
| 暂停登记或提交 | 公布预计时长 | `registry.resumed`；超过 14 天启动修订 | 第 49 条之一 |

暂停登记时，`registration.effective` 不写，描述照常提交和记承诺；暂停提交时，`description.submitted` 被拒，采集照常。

## 5. 承诺

内容与法规第 9 条逐项对应，这里定字段名和字节（NTJ，§2.1）【新提议 N4】。代码块里 `#` 后面是说明，不属于对象。

```
采集承诺 = SHA-256(NTJ({"fmt": "collection-commitment/1", "spec": "lenia-q16/1.0",
  "params": "sha256:…",   # 编码后的参数，以规范 §8.2 参数记录的哈希 hP 代表
  "n": 256, "state": "sha256:…",   # 环面尺寸；成体快照（规范 §8.4 状态记录）的哈希
  "trail": "sha256:…", "attr": "sha256:…", "nonce": "…"}))   # 探索路径（§3.2）；采集者的署名标识

名称子承诺 = SHA-256(NTJ({"fmt": "name-fields/1", "genus": "Anemorota", "epithet": "quadrifolia",
  "new_genus": {"note": "…"},   # 只在提议新属时出现
  "common": [{"lang": "zh", "text": "四叶风车"}, {"lang": "en", "text": "Four-leaf Windmill"}],
  "etymology": "…", "authority": "lin_hai", "nonce": "…"}))   # 另一个随机数

描述提交承诺 = SHA-256(NTJ({"fmt": "description-commitment/1", "prov": "prov:2026-11-7F3A2C",
  "state": "sha256:…", "name": "sha256:…",   # 名称子承诺
  "diagnosis": "…", "behaviour": "…", "observations": ["obs:…"],   # 观察记录编号，升序
  "ai": [{"part": "behaviour", "help": "draft", "tool": "description-assistant/1.0.0"}],   # 空数组＝声明"没有"
  "attr": "sha256:…", "nonce": "…"}))

署名标识 = SHA-256(NTJ({"fmt": "attribution/1", "account": "<账户编号>", "salt": "<账户盐>",
  "nonce": "<所属承诺或事件的随机数>"}))
```

- **普查采集**的 `attr` 用 `{"fmt": "attribution/1", "census": "CEN-0003", "nonce": …}`，没有盐，公开后任何人都能核对。**工具采集**照常用玩家的署名标识，采集方式写在 `collection.committed` 的载荷里，随事件进链，事后改不了。
- **新属说明**放进名称字段。改选已有属只改名称字段，保留提交时间（法规第 8 条第 3 款、第 23 条第 3 款）。**改名重交**时，新的名称子承诺带出新的描述提交承诺，`description.name_resubmitted` 的载荷写明第一个承诺。
- **以人命名**的同意书不进承诺，存私密位置；条目只写 `eponym_consent: true`（法规第 26 条第 1 款）。
- 署名标识算不算个人数据 [待法务确认，法律备忘 Q24(d)]。

**什么时候公开什么**：采集和提交描述时，只公开承诺哈希。普查标本进入待描述库时，公开采集承诺原文、参数记录和状态记录。登记时，公开法规第 31 条第 3 款所列：描述提交承诺（改过名的，各版都公开）和登记所用名称字段的原文与随机数，相关采集承诺的原文与随机数，以及参数记录、状态记录和探索路径；被退回的名称只公开子承诺哈希。账户编号、账户盐和没有成立的描述的内容永不公开。

**为什么未登记标本连参数哈希也不公开**【新提议 N8】：规则点只有 7 个字段，全是小范围的整数（规范 §3）。β 取常见的一两圈时，合法组合粗算在 10^11–10^13 量级（暂定），用显卡穷举 SHA-256 反推出参数是可行的 [verify]。所以未登记标本的 hP 只出现在加了随机数的承诺里，私密阶段的事件载荷整体暂缓公开（§6.1）。

## 6. 只追加的事件日志

### 6.1 信封与链【新提议 N5】

```
{"fmt": "event/1",
 "seq": 20131,                            # 事件序号，连续，无空号
 "ts": "2026-11-02T14:05:12.000Z",        # 写入时的服务器时间
 "type": "description.submitted", "tv": 1,   # tv 是该类型载荷的版本
 "actor": {"kind": "player", "attr": "sha256:…"},
 "payload_hash": "sha256:…",              # SHA-256(NTJ(载荷))；载荷里一定有 128 位的 "nonce"
 "prev": "sha256:…",                      # 前一条的 hash；第 1 条写 "sha256:" 加 64 个 0
 "hash": "sha256:…"}                      # SHA-256(NTJ(去掉 hash 之后的信封))
```

- **单一写入者**：序号在写入时顺序分配。`ts` 取服务器时钟，保证随序号单调不减；时钟回拨时沿用上一条的值。**先后只看序号**，`ts` 只用于显示和署名年份（法规第 10 条第 3 款、第 27 条第 3 款）。
- **行为者**：`{"kind": "player", "attr": …}` 用每事件不同的署名标识。承诺事件用承诺里的那个，其他事件用载荷里的随机数另算，只看日志无法把同一个人的事件串起来（登记后署名表会公开谁是采集者和描述者）。其余写 `{"kind": "system", "id": "assay"}`（assay、dedup、census、registry、judge：悬赏的自动判定【待 SB14】）、`{"kind": "panel"}`（第一阶段的临时评审组【待 D-038】）和 `{"kind": "team"}`；具体是谁见裁决和署名表。
- **链只覆盖信封**，载荷由 `payload_hash` 绑定。载荷有三种状态：**公开**（导出时附在信封后的 `payload` 字段）；**暂缓**（只有哈希，到公开时点再附上原文，任何人都能核对它在那个序号之前就已存在）；**隐去**（曾经公开、按规则不再显示；导出行去掉 `payload`，加 `withheld` 原因代码，两者都在信封之外，哈希不变，链照样能核对）。隐去用于法规第 56 条第 3 款的违规原名和法律要求的情形。已发布的导出和快照收不回，CHANGELOG 如实说明。
- 信封里的 `type` 和 `ts` 总是公开，所以只在私密暂存区里发生、还不能让人知道"发生过"的事（如获准之前的文献导入，§3.3）不写日志。

### 6.2 每日链头

- 每天 00:00 UTC（暂定）取最后一条事件，发布 `{"fmt": "log-head/1", "date", "seq", "hash", "sig"}`。签名用 Ed25519【新提议 N6】。密钥管理见系统架构第 7 节：证书键签证书和悬赏判定结果，链头键签链头和导出清单【待 AR9】。浏览器能否直接验 Ed25519 [verify]。
- `keys.json`：`[{id, alg: "ed25519", use: "cert" | "log", public, not_before, not_after, status: active | retired | revoked, revoked_at, boundary_seq}]`；退役和吊销的键永不删除，旧签名永远能验。每次变化写 `key.published` 或 `key.revoked`（§6.3），所以 `keys.json` 是日志的投影【待 AR9、AR10】。沙盒的键带 `test-` 前缀，另有自己的 `keys.json`。
- 发布渠道（暂定）：网站 `/log/heads/`；推送到一个公开代码仓库；有余力时再交给第三方时间戳服务（如 RFC 3161 服务）[verify：可用的服务]。每次外部留存写一条 `log.anchored`。链头一旦留在外部，运营方改写它之前的任何记录都会被发现（防倒填）。核对优先权时，以已公开的链头之前的记录为准（法规第 9 条）。每周导出附上全部链头。

### 6.3 事件类型（第一阶段）

| 类型 | 载荷要点 | 载荷公开 |
|---|---|---|
| `collection.committed`、`description.submitted`、`description.name_resubmitted` | `commitment`；采集加 `method`、是否经链接；重交加第一个承诺 | 立即（承诺本身只是哈希） |
| `precheck.done`、`assay.{requested, certified, failed}`、`dedup.decided` | prov、结果、`cert` 或 `report`；`assay.failed` 带原因码 died / invalid_input / size_rule / engine_error / cancelled（只有 died 带证书）【待 AS15】 | 登记时或进入待描述库时；从未公开的永远暂缓 |
| `census.{started, ended, pooled}`、`novice_zone.published` | 运行、标本的承诺与原文；新手区 | 立即 |
| `description.{paused, resumed, shelved, lapsed, retracted}`、`literature_check.{started, concluded}`、`name_review.{started, returned, rejected}` | 描述、原因代码（违反哪一条） | 登记时；没有成立的永远暂缓（被退回的名称只按条款公开分类统计，法规第 30 条） |
| `review.{opened, vote, concluded}` | 见 §3.4 | 结案时；涉及未登记描述的，随登记 |
| `registration.effective` | LEN、承诺原文与随机数、名称字段、描述全文、`discloses`（随之公开的序号） | 立即 |
| `objection_period.ended`、`objection.filed`、`ruling.issued`、`appeal.{filed, ruled}`、`cure.{opened, completed, expired}` | 见 §3.4 | 立即 |
| `entry.status_changed`、`name.changed`、`common_name.{added, changed}`、`taxonomy.{recombined, genus_proposed, genus_decided, revised}`、`entry.split` | from、to、依据 | 立即；违规原名隐去 |
| `certificate.corrected` | 旧证书、新证书、原因码 `bug_fix` / `supplement`（补测，PRD F16 的"重新签发"）【待 AS16】 | 立即 |
| `literature.imported`、`literature_reference.created`、`reference_specimen.attached` | 见 §3.3 | 立即；`literature.imported` 到 G-文献可见才写（§3.3） |
| `keen_eye.{flagged, credited}`、`citation.recorded`、`bounty.{published, amended, opened, submitted, closed}`、`judge.result` | 见 §3.5、§3.6。`bounty.submitted` 只用于要人提交的悬赏；登记时自动判定的不写，`judge.result` 的行为者写 system；更正也是一条新的 `judge.result`，带 `supersedes`【待 SB14】 | 慧眼标记在记功时；其余立即 |
| `weekly_report.published` | `WR-…`、`weekly-report/1` 对象的哈希；更正版带 `supersedes`，不另设更正事件；行为者写 team【待 AI14、AI15】 | 立即 |
| `key.{published, revoked}` | 键编号、公钥、用途、生效时间；吊销加原因、`revoked_at` 和 `boundary_seq`【待 AR9、AR10】 | 立即 |
| `registry.{paused, resumed}`、`ai_declaration.corrected`、`sanction.ruled`、`code.revised`、`schema.changed`、`log.anchored` | 范围、理由（账户层面的措施不写）；法规修订、格式变更、外部留存 | 立即 |

**不进日志**：账户和登录、同意和条款接受记录、好奇心事件、运营遥测、观察志、个人测绘、野外笔记、配额、分享记录。

### 6.4 撤回同意与删除账户

只追加的日志和删除权的冲突，按下面处理【新提议 N7，待 D-048，[待法务确认] Q24】：

1. 删除账户的私密字段（邮箱、认证、地区等），销毁账户盐。此后所有署名标识都无法再关联到这个人。
2. 私密载荷（没有成立的描述、采集箱里没公开的标本）按账户加密存放，删除时销毁密钥（加密粉碎；单人时的做法见 §1）。链只含哈希，不受影响。审核中的描述改为失效（§4.2）。加密粉碎和留在链里的哈希是否满足删除义务 [待法务确认，Q24]。
3. 署名表里这个人的显示名，以后的导出改为"匿名贡献者"（D-048 选项 A）。
4. 已登记学名后的命名者署名（`authority`）在登记事件的公开载荷里，按 D-048 选项 A 保留。法律意见要求改的，用"隐去"处理以后的公开视图；已发布的快照收不回（法律备忘 Q24(b)）。

**撤回同意**只影响好奇心数据（§3.7）。登记所需的出处在 0 级也保留（D-012），不受影响。

## 7. 内容寻址与分享链接

**内容存储**：`/cas/sha256/<hex>` 返回规范字节（规范 §8 的记录或 NTJ 对象）。传输可以压缩，哈希只对解压后的规范字节计算【待 D-026】。私密对象不提供，只有一个例外：玩家分享未描述的标本时，它的参数记录和状态记录凭哈希可取，不列入任何公开索引（哈希只在分享出去的链接里）。

**链接格式**【新提议 N12】（`<host>` 待定名，【待 D-013】）：

| 链接 | 形式 |
|---|---|
| 条目 | `https://<host>/LEN-004127`；异名跳到有效名并注明 |
| 条目的历史版本 | `https://<host>/LEN-004127?at=ev:21500`，按该序号之前的事件重算 |
| 标本（含未描述的） | `https://<host>/s/1#p=<参数记录>&n=256&h=<状态哈希>&z=<压缩的状态记录>` |
| 事件、对象、导出 | `/ev/20131`；`/cas/sha256/<hex>`；`/CEN-0003`、`/RUL-2026-0007` 等；`/export/2026-W46/` |
| 嵌入（预留） | `<iframe src="https://<host>/embed/1/LEN-004127">`；标本用 `/embed/1/s#…` |

- `/s/1` 的 1 是链接格式版本。旧版本永远能解析；新格式用 `/s/2`。
- 状态放在 `#` 之后，浏览器不把它发给服务器。`p` 是 72 字节参数记录的 base64url，总是内联，规范版本就在记录里。`z` 是状态记录的 deflate-raw 压缩再 base64url（浏览器自带的 CompressionStream [verify：目标浏览器都支持]）；压缩后不超过 6 KiB（暂定，第一阶段实测）才内联，否则省略 `z`，按 `h` 从内容存储取。解压后必须核对 `h`，不一致就拒绝运行。
- 分享未描述标本前的提示，以及野外笔记默认隐藏参数，见法规第 11 条和 D-015 建议。经链接得到的标本，探索路径的 `seed` 记为 link（法规第 11 条第 3 款）。编号和哈希都不依赖域名；换名时旧域名永久重定向，定名前先用中性域名（暂定）。
- **嵌入**：PRD 第 4 节把嵌入代码推迟到 alpha 首版之后（新提议），本文先定格式，路径 `/embed/1/` 先保留。实现时用单线程的 WASM 参考实现（第一阶段不用线程【待 D-037】），不需要跨源隔离（COOP/COEP），能放进别人的网页。默认整数精确运行；跑不动时是降为浮点预览并标明，还是按交接规格原则 6（对外只用整数帧）只慢放或显示静态整数帧（客户端性能第 14 节第 1 条），待创始人定。
- **降级**：遇到不认识的规范标识，不得换别的版本运行（规范 §3）；查看器从归档里加载该版本的引擎（GDD §14.2(6)），加载不了就只显示编号、哈希和静态图，并说明原因。遇到不认识的链接格式版本，显示原始片段和说明页链接，不猜。

## 8. 每周导出与 DOI 快照

### 8.1 每周导出包【新提议 N13】

每周一 00:00 UTC（暂定）截止，48 小时内（暂定）发布，全量。第一次公开导出要等 PRD §2.3 的 G-导出（D-018 定案，D-035、D-047 拍板）。包名 `catalog-export-2026-W46`（ISO 周）。发布后不替换：发现错误就在下一包更正，写进 CHANGELOG。每周包在线保留 12 个月（暂定），DOI 快照永久（§8.5）。`specimens/` 里的文件按哈希命名，各期相同的文件在托管端只存一份，每期只有新增的占空间（预算见[团队、预算与资助计划](../product/team-budget-funding.md) §3.3）。

| 文件 | 内容 | 许可 |
|---|---|---|
| `MANIFEST.json`；`README.md`、`DATASHEET.md`、`CHANGELOG.md`；`LICENSES/` | 清单（§8.2）；用法、数据说明书（§8.5）、本周变化；CC0-1.0 和 CC-BY-4.0 全文，选 A″ 时加 MIT 全文和 Chan 的版权声明 | 清单 CC0；项目文字 CC BY 4.0【新提议 N14】 |
| `entries.jsonl.gz`、`entries.csv` | 每个发出过的 LEN 一行，各种状态都在（§8.3）；CSV 是扁平摘要（编号、状态、学名、署名、规则点整数、N、状态哈希、主要测量、指标标记），UTF-8，RFC 4180 | CC0 |
| `descriptions.jsonl.gz`、`commitments.jsonl.gz` | 登记描述的全文、名称字段、AI 声明、观察记录（粗化）；已登记条目的承诺原文和随机数 | 文字 CC BY 4.0，其余 CC0 |
| `specimens/`、`trails.jsonl.gz`、`certificates.jsonl.gz`、`dedup.jsonl.gz` | 规范 §8 记录，按哈希命名（`<hex>.parm`、`<hex>.stat.gz`），轨迹分段列表；核和查找表由参数重算（规范 §9.1），不附。已公开的探索路径、证书（含签名）、查重报告 | CC0 |
| `reviews`、`objections`、`rulings`、`taxonomy`（均为 `.jsonl.gz`） | 评审、异议、裁决、申诉；属、修订和组合历史 | 理由文字 CC BY 4.0，其余 CC0 |
| `census/`、`novice_zones.jsonl`、`bounties.jsonl`、`judge_results.jsonl`、`judge_specs/`、`citations.jsonl`、`behaviour_classes.jsonl` | 普查运行、待描述库的普查标本、全局测绘格；新手区；悬赏与判定；判定器规格原文和测试向量【待 SB14】；引用；行为类别 | 问题、署名承诺和定义文字 CC BY 4.0；判定器规格和测试向量同测试向量的许可【待 D-047】；其余 CC0 |
| `credits.jsonl`、`events/YYYY-MM.jsonl.gz`、`log_heads.jsonl`、`keys.json` | 署名表（§3.1）；公开视图的事件日志，从第 1 条起，按月分卷；全部每日链头；签名公钥 | CC0 |
| `literature/` | 文献条目的数据；选 A′、A″ 时单独成目录，附各自的许可（法律备忘 §3.4 第 4 点） | 文 |

**不导出**：好奇心数据【待 D-035】；运营遥测【待 D-039】；私密字段；审核中的和没有成立的描述；玩家采集箱；观察志（只给计数）；野外笔记；账户与同意记录；涉及 Chan 物种的内部回归向量（D-045）；名称审核用的内部文献名单和对照参数表（法规第 22 条第 4 款、第 45 条第 4 款）；G-文献可见之前的文献数据【待 D-040】。图版不进导出，由生成器从数据重算；图版的许可人和形式见法律备忘 Q16。

### 8.2 清单

```
{"fmt": "export-manifest/1", "export_id": "2026-W46", "created_at": "…",
 "cut": {"seq": 21500, "hash": "sha256:…", "ts": "…"},   # 等于本包最后一条事件
 "previous": "sha256:…",                                 # 上一包清单的哈希，导出包也连成链
 "schemas": {"entry": 1, "event": 1, "…": 1}, "specs": ["lenia-q16/1.0"],
 "engine": {"version": "ref-1.2.0", "source": "swh:1:rev:…"},
 "files": [{"path": "entries.jsonl.gz", "sha256": "sha256:…", "bytes": 0, "records": 0, "schema": "entry/1", "license": "CC0-1.0"}],
 "excluded": ["curiosity", "private", "in_process"],
 "sig": {"alg": "ed25519", "key": "k1", "value": "…"}}   # 对去掉 sig 的清单的 NTJ 字节签名
```

### 8.3 条目（导出投影）

导出的投影不参与哈希，可以有 null，测量值写成 JSON 数字（从证书的十进制串原样转写）。下例沿用 GDD §14.5 的虚构示例。

```yaml
schema: entry/1
len: LEN-004127
prov: "prov:2026-11-7F3A2C"
as_of_seq: 21500
status: valid                 # valid | synonym_of | withdrawn | literature | literature_unreproduced | literature_reference
synonym_of: null              # status 为 synonym_of 时填；resolved_valid 是沿异名链解析出的有效名
resolved_valid: LEN-004127
registered_at: 2026-11-03T02:10:00.000Z   # 法规第 31 条；registered_seq 是登记事件的序号
registered_seq: 20877
objection_period_ends: 2026-12-03T02:10:00.000Z
objection_cleared_at: null
universe: {spec: lenia-q16/1.0, params: "sha256:…", kf: 1, gf: 1, R: 13, T: 10, mu_q: 2120, sigma_q: 310, beta: [[1, 1], [1, 2]]}
type_specimen: {n: 128, state: "sha256:9f2c…", kernel: "sha256:c3b1…", growth_lut: "sha256:5e7a…", cert: "cert:sha256:…",
                trajectory: {interval: 256, steps: 20000, chain: "sha256:41aa…", segments: "sha256:9c0d…"}}   # birth_reproducible 推迟（PRD 第 4 节）
name: {genus: Anemorota, epithet: quadrifolia, authority: lin_hai, year: 2026, eponym_consent: null,
       common: [{lang: zh, text: 四叶风车}, {lang: en, text: Four-leaf Windmill}]}
combinations: [{genus: Anemorota, from_seq: 20877}]
credits: {collector: {method: manual, display: lin_hai}, describer: {display: lin_hai}, keen_eye: null}   # 普查标本写 {method: census, run: CEN-…}
ai_assistance: [{part: behaviour, help: draft, tool: description-assistant}]
assay_summary: {classes: [rotator], period: 24, speed: 0.0, symmetry: C4, self_repair: 0.71, discretization_range: {R: [10, 30], T: [5, 40]}, platonic: true}
dedup: {report: "dedup:sha256:…", decision: auto_new, model: "…", nearest: [{ref: LEN-001876, distance: 0.62}]}
provenance:
  collection: {seq: 20114, ts: 2026-11-02T13:41:07.000Z, commitment: "sha256:7d0e…", trail: "trail:sha256:7c1e…"}
  description: {seq: 20131, ts: 2026-11-02T14:05:12.000Z, commitment: "sha256:a91f…"}
metrics_flags: {in_novice_zone: false, in_census_coverage: true, census_collected_before: false}   # 登记时冻结【待 D-043】
reference_specimens: []
range: {samples: 37}          # 分布区地图推迟到 alpha 之后（GDD §19.1）
citations: {origin: 1}        # 第一阶段只记起点引用（§3.6）
observers: "<5"
license: {data: CC0-1.0, text: CC-BY-4.0, plates: CC-BY-4.0}   # 【待 D-018】；plates 待法律备忘 Q16；文献条目按 A′、A″ 覆盖
history: [20114, 20131, 20877]
```

`metrics_flags` 在登记时算好并冻结，供人类独有发现率扣除新手区、限定普查覆盖区和排除工具采集（看 `credits.collector.method`）【待 D-043】（法规第 19、51 条），避免事后改口径【新提议 N19】。

### 8.4 异名、撤回和改名怎样表示

- **异名**：行还在，`status: synonym_of`，`synonym_of` 写被指向的编号，`resolved_valid` 写沿链解析出的有效名。异名原来的模式标本出现在有效名的 `reference_specimens` 里（法规第 36 条第 2 款）。
- **撤回**：行还在，`status: withdrawn`，附裁决编号和原因代码；名字永久占用（法规第 40 条）。
- **违规改名**：`name` 只写新名，`name_changes` 列出裁决编号和时间，不写原名；对应事件的载荷在公开视图隐去（法规第 56 条第 3 款）。
- **新组合**：`combinations` 按序号列出历次属名；署名的括号写法见法规第 39 条第 2 款。
- **空号**：LEN 不会有空号（§2.2）；万一因故障出现，CHANGELOG 写明，不补发、不复用。

### 8.5 DOI 快照、数据说明书与工具

**DOI 快照**（GDD §14.9）：某一期每周包，加全部内容寻址对象、公开视图的完整日志、全部链头、参考实现源代码的 SWHID 和构建产物、规范正文与公开测试向量（许可【待 D-047】）、查重模型的权重和代码【待 D-034】。文件和清单格式与每周包相同，研究者用同一套工具读。存 Zenodo，整个系列一个概念 DOI，每次一个版本 DOI [verify：Zenodo 的版本机制]；镜像到互联网档案馆。

**第一个 DOI**【新提议 N15】：PRD 第 4 节把月度 DOI 快照移出第一阶段（新提议），本文按此写。第一阶段内，引用写导出编号加清单哈希（如 `2026-W46` + `sha256:…`）。第一个 DOI 用第一阶段最后一期每周导出（PRD P1-W8）做，在 GDD §18.1 的统计时点（第一阶段结束后 30 天）之前存档，供研究者引用（PRD 第 1 节 V3）；此后按月。前提：D-018 已定案；D-048 和 Q24 已有结论（快照发出就收不回）；文献数据按 D-040。前提不满足就推迟 DOI，每周导出照常。开工后、alpha 之前，先用测试沙盒数据生成一份**样例导出**（编号用 `TST-`，标 `sample: true`，不含任何玩家数据），请顾问研究者看格式；这也是访谈问题 Q-07 的输入。

**数据说明书**（`DATASHEET.md`）写：用途、组成、采集过程、各文件的许可、怎样复核、维护方式，以及**已知偏差**：

1. 新手考察区是富矿，区内登记偏多，用 `in_novice_zone` 扣除【待 D-043】；普查选区由团队和顾问选（D-008），覆盖不均，用 `in_census_coverage` 限定。
2. 一切只在 lenia-q16 的格点和合法 R、T、N 下成立；"柏拉图型"是操作性标签，不等于 Davis（2024）的判据；只在浮点预览里稳定的结构（幻影）不在图鉴里【待 D-029】。
3. `literature_reference` 没有数据；文献种附近的候选靠人工对照，查重较弱（法规第 45 条）；早期查重模型较弱，异名会滞后被发现（法规第 35 条）。
4. 邀请制内测，人数按 PRD 的批次（GDD §19.1 的目标是 200–500 人），偏生命游戏和 Lenia 爱好者；每人每周描述配额相同（D-005）；推荐会影响玩家去哪里看（GDD §11.3）。先后按日志序号，时间戳只供显示。

**研究者工具**放在代码仓库（许可【待 D-032】）：核对清单、签名和文件哈希；核对事件链和每日链头；用公开的原文重算每个已登记条目的承诺；从参数重算 hP、hK、hG；用参考实现复跑模式标本，核对轨迹链（可以抽样）；从日志重建条目投影，与 `entries.jsonl` 比对。最小的读取和核对示例：

```python
import gzip, hashlib, json
ntj = lambda o: json.dumps(o, sort_keys=True, separators=(",", ":"), ensure_ascii=False).encode()  # §2.1 [verify]
sha = lambda b: "sha256:" + hashlib.sha256(b).hexdigest()
root = "catalog-export-2026-W46/"
man = json.load(open(root + "MANIFEST.json", encoding="utf-8"))
for f in man["files"]:                                        # 1. 文件哈希
    assert sha(open(root + f["path"], "rb").read()) == f["sha256"], f["path"]
prev, n = "sha256:" + "0" * 64, 0                             # 2. 事件链（签名验证要另装 Ed25519 库）
for vol in sorted(f["path"] for f in man["files"] if f["path"].startswith("events/")):
    for line in gzip.open(root + vol, "rt", encoding="utf-8"):
        ev, n = json.loads(line), n + 1
        env = {k: ev[k] for k in ("fmt", "seq", "ts", "type", "tv", "actor", "payload_hash", "prev")}
        assert env["seq"] == n and env["prev"] == prev and sha(ntj(env)) == ev["hash"], ev["seq"]
        assert ev.get("payload") is None or sha(ntj(ev["payload"])) == ev["payload_hash"], ev["seq"]
        prev = ev["hash"]
assert prev == man["cut"]["hash"]
entries = [json.loads(l) for l in gzip.open(root + "entries.jsonl.gz", "rt", encoding="utf-8")]  # 3. 读条目
print(sum(e["status"] == "valid" for e in entries), "valid entries")
```

## 9. 版本与迁移【新提议 N16】

1. **发布了就不改**（§1 原则 2）。投影可以重算；事件、承诺、证书、报告、链头和导出包不改。
2. **规范版本**：条目永远绑在登记时的规范版本上，不迁移到新版本。新版本规范下是新条目，对应办法按法规第 22 条第 6 款，在新版本开放登记前公布。
3. **被哈希的对象**：任何字段或含义的变化都换新的 `fmt` 主版本。旧对象保持旧版本，永远可以核对。承诺、信封、链头和编号的格式在**第一条公开事件之前**冻结，不是第一条登记之前：alpha 期间的采集承诺（以及 alpha 之前公布的普查和新手区事件）要在登记时原样打开核对，写下之后就不能改。在那之前，沙盒里的事件随测试数据一起丢弃。
4. **事件**：载荷变化时升 `tv`。旧事件保持原来的 `tv`；读日志的程序要支持出现过的每个版本，各版本的载荷定义都留在本文附表里（第一阶段补）。
5. **导出投影**（`entry/1` 等）：只加可选字段时不升版本，读者必须忽略不认识的字段；删字段、改名或改含义时升主版本，新旧两版并行导出至少 8 周（暂定；单人时 4 周）。每次变更写 `schema.changed` 事件和 CHANGELOG。
6. **链接格式**和**编号格式**一经使用永不作废。**哈希算法**要换时（D-021 的复审条件），随新规范或新 `fmt` 换，旧哈希仍作标识。**私密库**的表结构可以自由迁移，只要能从日志重建出相同的投影。

**GDD §14.5 示例与本文的对应**：

| GDD §14.5 | 本文 | 变化 |
|---|---|---|
| `id`；`status`（含 `synonym_of:LEN-…`） | `len`，登记前只有私密的 `prov`；`status` 加 `synonym_of`、`resolved_valid` | 拆字段；字符串写法保留为显示形式 |
| `registered_at`；`objection_period_ends`（日期） | 同名，加 `registered_seq`；改为时刻，加 `objection_cleared_at` | 加序号；便于按 §18.1 统计 |
| `universe.kernel`、`growth`、`discretization`、`generated`；`type_specimen.window` | 规范编码的整数（kf、gf、R、T、mu_q、sigma_q、β 分数对）加 `params`（hP）；`type_specimen.kernel`、`growth_lut`、`n` | 十进制只作显示（规范 §3）；生成结果移到模式标本；只有 N×N（规范 §4） |
| `trajectory_hashes {every, steps, chain, segments}` | `trajectory {interval, steps, chain, segments}` | 改名；定义分段列表（§3.2） |
| `names.authority`（"lin_hai, 2026"）；`credits.*`、`ai_assistance` | `name.authority` 加 `name.year`；`credits.*.display`（来自署名表）加 `method`；`ai_assistance` 分项声明 | 显示形式不变；显示名可变为"匿名贡献者"【待 D-048】；法规第 17 条第 3 款 |
| `assay.*`；`dedup` | 独立的证书加 `assay_summary`；`dedup` 加 `report`、`model` | 证书单独存放、签名；模型版本【待 D-034】 |
| `provenance.*_at`、`*_commitment`、`exploration_path`、`birth_reproducible` | `provenance.collection`、`description` 的 `{seq, ts, commitment}`，路径在 `collection.trail`；`birth_reproducible` 推迟 | 先后按序号（法规第 10 条第 3 款）；路径不记墙钟时间（N9）；PRD 第 4 节 |
| `citations`；`license` | 由引用记录算出；条目级 `license` 加导出包按文件标许可 | 只读投影；文献条目可以覆盖 |
| — | 新增 `schema`、`prov`、`metrics_flags`、`combinations`、`name_changes`、`reference_specimens`、`observers`、`history`、`as_of_seq` | |

---

## 需要新开的决策条目

依赖的提议条目已在正文就地标出：D-013、D-015–D-019、D-021、D-024、D-026、D-027、D-029–D-040、D-042–D-045、D-047、D-048。下面是本文新提的选择，建议在决策日志里开条目。拍板人：创始人（暂代后端；到岗后可提修改）。截止建议：默认"第一阶段开工前"。N1–N6、N8、N9、N12、N16 最晚在**第一条公开事件之前**（alpha 开放或首次公布普查、新手区，以先到者为准，§9 第 3 条），不是第一条登记之前；N19 随指标口径在 G-邀请 之前冻结；N13 在第一次公开导出前（G-导出）；N15 在第一阶段结束前。

| 编号 | 条目 | 本文的建议 | 相关 |
|---|---|---|---|
| N1 | 规范化 JSON 与通用编码 | RFC 8785 子集（§2.1）；浮点按 ECMAScript 写法写成字符串；对象数组的排序；轨迹分段列表格式。法规第 9 条写"用 D-026 的序列化"，D-026 只管状态和参数的二进制记录，这里补上承诺和事件 | D-021、D-026、法规第 9 条 |
| N2 | 永久标识不含暂定名 | `fmt` 标签、包名、文件名都不用产品名 | D-013 |
| N3 | 编号格式与发号 | prov 随机 6 位十六进制、重复重抽；LEN 单一写入者、无空号、无保留号段；其他前缀，地名预留（§2.2、§3.6） | D-030、D-042 |
| N4 | 承诺的构造 | §5 的字段；hP 代表参数；普查的署名标识；新属说明放进名称字段 | D-001、D-015、法规第 9 条 |
| N5 | 事件日志的信封与链 | 链只覆盖信封；载荷哈希加随机数；公开、暂缓、隐去三态；每事件伪名；`actor` 的写法；还不能让人知道"发生过"的事不写日志 | 法规第 9、10 条 |
| N6 | 每日链头的发布与签名 | 每天 00:00 UTC；Ed25519；公开仓库，有余力加时间戳服务 | 法规第 9 条 |
| N7 | 删除账户与只追加日志 | 销毁账户盐；加密粉碎私密载荷（单人时直接删行、备份过期）；审核中的描述失效；公开视图可以隐去 | D-048、Q24 |
| N8 | 未登记阶段的公开范围 | 不公开裸的参数哈希；私密阶段的载荷暂缓；分享的标本凭哈希可取 | 法规第 9、11 条 |
| N9 | 探索路径不记墙钟时间 | 只记模拟步数和操作顺序；0 级下步数是否粗化随 D-039 | D-012、D-035、D-039 |
| N10 | 测绘、观察志和观察记录的公开边界 | 个人迷雾是无序位图，0 级的去留随 D-039；全局只有匿名计数；观察志只导出计数，<5 抑制；观察记录粗化到日和分钟 | D-039 |
| N11 | 没有成立的描述和没通过鉴定的送检的保留期 | 搁置后失效的描述保留到账户注销；其余描述 12 个月后删内容；没通过鉴定的送检保留 12 个月或到账户删除（团队、预算与资助计划 §3.3 要本文定） | 法规第 38 条、D-027、隐私文档 |
| N12 | 分享链接与嵌入 | `/s/1#…`；内联上限 6 KiB；deflate-raw；`?at=ev:`；嵌入只定格式（PRD 第 4 节推迟），单线程 WASM；未知版本的降级 | D-013、D-037 |
| N13 | 每周导出包 | §8.1 的文件、截点和保留；JSONL 为准、Parquet 推迟；含待描述库的普查标本，不含图版和内部文献名单 | D-018、D-035、D-047 |
| N14 | 字段级许可 | 人写的自由文字和团队写的说明文字 CC BY 4.0，其余 CC0 | D-018、Q15、Q16 |
| N15 | DOI 快照与第一个 DOI | 组成见 §8.5；第一阶段不做（与 PRD 第 4 节一致）；第一个 DOI 用第一阶段最后一期导出，在统计时点前存档；先发样例导出 | D-018、D-048、D-047、D-011 |
| N16 | 版本与兼容规则 | §9；`status` 拆字段；格式在第一条公开事件前冻结 | — |
| N17 | 评审逐票公开 | 评审人按本人同意署名 | D-038 |
| N18 | 文献条目的状态转换 | 导入先在私密暂存区，G-文献可见时才写事件、分配 LEN；升级为导入时沿用 LEN；literature 与 literature_unreproduced 随证书更正互转 | D-040、D-042、D-027 |
| N19 | 指标标记在登记时冻结 | `in_novice_zone`、`in_census_coverage`、`census_collected_before` | D-043、D-011 |
| N20 | 野外笔记页的存储 | 客户端生成；只在保存或分享时上传；不导出 | D-015 |
| N21 | 查重报告的时点 | 鉴定后、提交描述时、登记前各一次 | dedup v1 |

## 变更记录

| 日期 | 版本 | 变更 |
|---|---|---|
| 2026-10-21 | v0 底稿 | 首次起草：实体与字段、三张状态机、承诺构造、事件日志与链头、链接、每周导出与 DOI 快照、版本规则、GDD §14.5 对照；新提议 N1–N21 |
| 2026-10-21 | v0 底稿（审查后修订） | 按对抗式审查修订，节号不变：第一个 DOI 改到第一阶段结束后、嵌入代码只定格式（与 PRD 第 4 节一致）；格式冻结点改为第一条公开事件之前；分享的未描述标本凭哈希可取；文献导入到 G-文献可见才进日志；补文献对照、按第 38 条恢复、`cure.expired` 等状态转换，证书更正改走撤回流程；补没通过鉴定的送检的保留期、地名的预留；浮点写法和对象数组排序写死；原则 4 改为只说信封；好奇心类型、引用类型按 PRD；单人降级表去掉功能范围；补提议标记和 [待法务确认]；链接 PRD 计划路径 |
