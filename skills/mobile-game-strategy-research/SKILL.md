---
name: mobile-game-strategy-research
description: "海外手游战略研究方法论 skill。按'先判断再展开、不做泛泛综述、数据+产品拆解结合'的原则，引导对赛道/产品/趋势的结构化研究，产出 9 字段报告(结论/定义/案例/证据/逻辑/反证/机会/动作/验证)并收敛到战略结论。覆盖：产品案例拆解、趋势判断、战略结论、历史资料消化、评审日志。需要 Sensor Tower 数据时调用 sensor-tower-fetch-byok（net/gross 口径每次由用户确认）。编辑飞书交付物时调用 feishu-doc-editing。触发词：手游战略研究 / 研究一个手游产品 / 判断一个市场趋势 / 赛道研究 / 产品案例拆解 / 趋势判断 / 战略结论 / 历史资料消化 / mobile game strategy research。"
---

# 海外手游战略研究方法论

本 skill 封装一套海外手游战略研究方法论，用于对赛道、产品、趋势做结构化研究并产出可决策结论。**不负责取数**——需要 Sensor Tower 数据时调用 `sensor-tower-fetch-byok`（口径 net/gross 由用户确认）；**不负责飞书文档编辑**--编辑飞书交付物（专题报告/选题跟踪）时调用 `feishu-doc-editing`。⚠ **飞书是交付面**，专题报告/选题跟踪都在飞书维护，本地只存过程与快照。

## 1. 研究原则（必遵）

### 先判断，再展开
每个研究对象优先回答：
- 是否代表真实趋势？
- 对海外手游研发/发行有什么价值？
- 是否值得投入资源验证？
- 是立项机会、项目迭代、发行机会，还是素材/商业化参考？

### 不做泛泛综述
避免只输出"市场很大 / 用户在变 / 玩法融合 / AI 很热"。必须补：代表产品、数据证据、增长原因判断、反证与风险、对公司的可执行建议。

### 数据和产品拆解结合
ST 数据能证明：下载/收入趋势、国家差异、榜单变化、竞品表现、品类增衰。
不能单独证明：玩家为什么喜欢、增长是否来自玩法本身、买量是否可复制、长线留存是否健康、我们能否做成。
每个重要结论需结合：ST 数据 + 产品玩法拆解 + 版本节奏 + 买量素材 + 玩家反馈 + 同类对比。

## 2. 输出形态（飞书长报告为主，9 字段为案例可选）

实际产出以**飞书长报告**为主（专题报告/选题跟踪，含数据表 + whiteboard 图表 + 阶段判断 ol），飞书是交付面、本地存过程与快照。**9 字段**（详见 `templates/输出结构说明.md`）是**单品拆解/案例卡片**的可选结构化模板，非唯一标准；专题报告不强制 9 字段，按内容组织即可。9 字段如下（案例可选）：

1. 一句话结论
2. 定义（边界，含"不包括什么"）
3. 代表案例
4. 数据证据
5. 成立逻辑
6. 反证与风险
7. 对公司的机会
8. 建议动作（条件化：若公司有 X 能力→研发进入；否则发行借鉴）
9. 后续验证

附结论等级：A（深做）/ B（观察）/ C（低频）/ 仅观察。

## 3. 工作流

### A. 产品案例研究
1. 复制 `templates/产品案例模板.md`
2. 填基本信息 + **数据口径声明（net/gross）**
3. 拉 ST 数据（调 `sensor-tower-fetch-byok`，**先问用户口径**）
4. 拆解玩法/系统/商业化/增长原因
5. 输出条件化战略判断 + 下一步验证

### B. 趋势判断
1. 复制 `templates/趋势判断模板.md`
2. 明确趋势定义边界
3. 找 3-5 个代表产品 + 反例
4. 拉下载/收入/榜单/国家数据（调 ST skill，确认口径）
5. 判断趋势可信度、商业价值、可执行性
6. 收敛到战略结论（`templates/战略结论模板.md`）

### C. 历史资料消化
1. 复制 `templates/历史研究资料消化模板.md`
2. 提取已有结论，标记已验证/待验证/可能过时
3. 映射到研究框架
4. 补 ST 数据或产品拆解
5. 沉淀到结论

### D. 评审日志（反证与校验）
- 结论存疑、数据口径变更、方法论调整时，复制 `templates/评审日志模板.md` 记一条
- 编号递增，存对应方向 `评审日志/`，建 INDEX 索引

## 4. 判断标准

### 趋势成立（至少满足 3 条中 2 条）
1. 多个产品复现，非单一爆款
2. 至少一个核心海外市场有下载/收入验证
3. 机制与增长有合理因果，非纯 IP/买量/渠道驱动
4. 后续产品/素材在模仿（行业扩散）
5. 玩法结构可复制，非高度依赖特定团队/IP

### 战略价值维度
市场空间 / 增长质量 / 研发可行性 / 发行可行性 / 差异化 / 与现有项目关联 / 风险

### 战略结论四档
立即推进 / 小规模验证 / 持续观察 / 暂不投入

## 5. 证据要求

- **必需**：ST 下载/收入数据、代表产品列表、产品机制拆解、国家/市场分布
- **强建议**：类别排名变化、活跃/留存迹象、广告素材/SOV/下载来源、版本节奏、玩家评论、反例产品

## 6. 与取数 / 作图 skill 的联动

需要 ST 数据时：
1. 调用 `sensor-tower-fetch-byok` skill
2. **先问用户口径：net（ST 原始净收入，不 ÷0.7）还是 gross（÷0.7 还原毛支出）**——每次必确认，不替用户决定
3. 给产品 + 日期区间，拉下载/收入（可选 DAU）
4. 在研究产出里声明所用口径
5. 口径基准见 `sensor-tower-fetch-byok` 引用的 `数据口径说明.md`

ST 能力速查：

| 问题 | Endpoint |
|---|---|
| 下载/收入趋势 | `/v1/{os}/sales_report_estimates` |
| 产品元数据 | `/v1/{os}/apps` |
| 活跃用户 | `/v1/{os}/usage/active_users` |
| 类别排名 | `/v1/{os}/category/category_history` |
| 买量素材 | `/v1/{os}/ad_intel/creatives` |
| 广告 SOV | `/v1/{os}/ad_intel/network_analysis` |
| 下载来源 | `/v1/{os}/downloads_by_sources` |
| 版本更新 | `/v1/{os}/app_update/get_app_update_history` |

### 作图联动（chart-style）

9 字段的"数据证据"经常需要图表。需要作图时调用 `chart-style` skill（咨询风格）：

- **触发**：趋势图 / 对比图 / 瀑布图 / 构成图 / 散点等任何"把数据画出来"的请求
- **风格要求（不可省）**：
  - insight headline 点出"So What"（如"靶心-like 占肉鸽 Top3 收入 60%"，而非"收入按方向"）
  - minimal ink：无 3D / 阴影 / 底色 / 渐变
  - direct labels：直接标数据点，不用图例
  - 底部 source line 注明数据来源 + 口径（如 `Source: Sensor Tower, net 口径；分析`）
- **图表选型**：趋势→line、对比→横向 bar、构成→Mekko/堆叠、变化→waterfall、两变量→scatter
- 配色/版式严格按 chart-style 的 references（color-palette / chart-types / slide-anatomy）
- **飞书 SVG 画板图表**优先按 `../chart-style/作图规范_飞书适配.md`（飞书适配版）执行，与 chart-style 通用规则冲突时以飞书版为准
- 图表属过程产出，最终 slide 放 `04_输出/`

### 飞书编辑联动（feishu-doc-editing）

专题报告/选题跟踪等交付物在飞书维护，编辑时调用 `feishu-doc-editing` skill：

- **核心循环**：`docs +fetch --detail with-ids` 取 block_id -> `block_insert_after`/`block_replace`/`str_replace` 写 -> 再 fetch 验证（⚠ `ok:true` ≠ 生效，必验证）。
- **图表**：Python 生成 SVG -> `whiteboard +update --input_format svg` 上传画板（保 token）-> 文档内嵌 `<whiteboard token="..."/>`。
- **评论**：`drive file.comments list --file-type docx` 读评论；`contact +get-user` 解析评论者 open_id。
- **classifier 阻塞**：用 Write 写 python 脚本 + 用户**半角 `!`** 跑绕过（全角 `！` 不执行）。
- 踩坑速查（fragment 泄漏 / `@file` 仅 `--content` / `block_replace` 触发 ol seq 重编号 / `ok:true`≠生效）见 `feishu-doc-editing` SKILL.md。

## 7. 研究框架（如何搭一个赛道/趋势框架）

`frameworks/` 下有两个示例框架（肉鸽赛道、市场新趋势），展示如何为某个研究方向搭建：

**核心问题 → 分类体系 → 判断标准 → 证据要求 → ST 用法 → 输出形态 → 待验证假设（含回填闭环）**

新研究方向时，参考这两个示例搭自己的框架；假设随研究进展回填验证结论（状态：待验证/已验证/已推翻 + 证据）。

## 8. 安装与分发

| 范围 | 放哪 | 效果 |
|---|---|---|
| 仓库级 | 仓库内 `.claude/skills/mobile-game-strategy-research/` | 在该仓库开 Claude Code 即可用 |
| 用户级/全局 | `~/.claude/skills/mobile-game-strategy-research/` | 本地任意项目可用 |

分发：整个文件夹拷给对方。ST 数据需对方自配 key（用 `sensor-tower-fetch-byok`，BYOK）。

## 9. 不做的事

- 不负责取数（调 `sensor-tower-fetch-byok`）/ 不负责飞书编辑（调 `feishu-doc-editing`）
- 不替用户决定 ST 口径（net/gross 每次确认）
- 不做泛泛综述（专题报告须有数据证据 + 反证与风险；案例卡片可用 9 字段，非强制）
- 不下无证据结论（必需证据组合）
