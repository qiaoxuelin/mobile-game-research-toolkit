# Sensor Tower 一键取数 · Claude Code Skill（自带 Key / BYOK 版）

一个 [Claude Code](https://claude.com/claude-code) skill：对 Claude 说一句「拉某某游戏 2024 年的下载和收入」，
它就会调 **Sensor Tower API** 把数据拉下来、套一套估值口径调整、导出成多 sheet Excel。**不用手动从网页导出 CSV。**

> **本版本不含任何 API key**——你需要用**自己的** Sensor Tower API token。配置一次即可。

---

## 能做什么

- 按**产品名 + 日期区间**直接取数（下载量 / 收入，可选 DAU）。
- 自动套一套投资评估口径（见下方「口径」），重算 RPD / ARPDAU。
- 输出 Excel 三张表：**明细 / 按 App 汇总 / 按地区汇总**。
- 多产品、多地区（默认 中 / 美 / 日 / 韩 / 其它）、月 / 周 / 季 / 日多种粒度。

## 前置条件

1. **Claude Code**（网页版 [claude.ai/code](https://claude.ai/code) 或本地 CLI 均可）。
2. **Sensor Tower 账号 + API token**：这是 Sensor Tower 的**付费数据**，需你自己的账号与相应数据订阅权限。
   token 在 Sensor Tower 后台的 **API / Integrations** 设置里生成。本 skill 只解决「用谁的 key」，**不绕过订阅**。
3. **Python 3 + 依赖**：`pip install pandas numpy openpyxl requests`（云端 Claude Code 环境通常已自带）。
4. 能联网访问 `api.sensortower.com`。

## 安装

把整个 `sensor-tower-fetch-byok/` 文件夹放到下面任一位置：

| 位置 | 效果 |
|---|---|
| 某个仓库内的 `.claude/skills/sensor-tower-fetch-byok/` | 在该仓库目录里开 Claude Code 时可用 |
| 本地 `~/.claude/skills/sensor-tower-fetch-byok/` | 本地**任意**项目都能用 |

```bash
# 全局安装示例
mkdir -p ~/.claude/skills
cp -r sensor-tower-fetch-byok ~/.claude/skills/
```

## 配置你的 API Key（必做一次，三选一）

优先级：环境变量 `ST_AUTH_TOKEN` > `config.local.json` > `config.json`。

1. **环境变量（推荐，最不易误提交）**
   ```bash
   export ST_AUTH_TOKEN=你的token
   ```
2. **本地配置文件**（不会被 git 提交）：复制示例再填入 key
   ```bash
   cd 这个skill目录
   cp config.local.example.json config.local.json
   # 编辑 config.local.json，把 auth_token 改成你的 token
   ```
3. **直接改 `config.json` 的 `auth_token`** —— ⚠️ 这个文件可能被提交，**别把 key 推到公共仓库**。

> 你的 key 只存在你自己的环境 / 本地文件里。`config.local.json` 已被 `.gitignore`，不会随仓库分享出去。

## 怎么用

### 方式 A：直接跟 Claude 说（推荐）

装好 skill、配好 key 后，在 Claude Code 里说一句即可，比如：

> 「拉 Genshin Impact 和 Honkai: Star Rail 2024 全年的下载和收入」

Claude 会：问清产品 / 日期 → 先确认匹配到的是正确的 App → 拉数 + 口径调整 → 给你 Excel 和要点。

### 方式 B：自己跑脚本

先写一个 `query.json`：

```json
{
  "products": ["Genshin Impact", {"name": "鸣潮", "unified_id": "<unified_app_id>"}],
  "countries": ["CN", "US", "JP", "KR", "OTHER"],
  "start_date": "2024-01-01",
  "end_date": "2024-12-31",
  "granularity": "monthly",
  "os": "unified",
  "include_dau": false
}
```

| 字段 | 说明 |
|---|---|
| `products` | 必填。字符串（按名搜索）或 `{"name","unified_id"}`（给 id 更稳、显示名更干净）。 |
| `countries` | 默认 `["CN","US","JP","KR","OTHER"]`。`"OTHER"` = 全球(WW) 减去其余列出的国家。 |
| `start_date`/`end_date` | 必填，`YYYY-MM-DD`。 |
| `granularity` | `monthly`（默认）/ `weekly` / `daily` / `quarterly`。 |
| `os` | `unified`（默认，iOS+Android 合计）/ `ios` / `android`。 |
| `include_dau` | `true` 则额外拉 DAU 并算 ARPDAU（慢一点）。 |

然后：

```bash
# 先确认产品匹配（也用来检查 key 是否配好）
python3 scripts/fetch.py query.json --resolve-only
# 正式拉数 + 调整 + 出 Excel
python3 scripts/fetch.py query.json [输出.xlsx]
```

输出默认在 `query.json` 同目录、文件名加 `_ST_adjusted.xlsx`。

## 输出

Excel 三张 sheet：

- **明细**：每行 = 产品 × 地区 × 期；含原始与调整后的 Downloads / Revenue / RPD（选了 DAU 则含 DAU / ARPDAU）。
- **按App汇总**：按产品聚合，按调整后收入降序。
- **按地区汇总**：按地区聚合。

## 口径（调整规则）

| 字段 | 中国(CN) | 其它地区 |
|---|---|---|
| Downloads | × 4 | 不变 |
| Revenue | ÷ 0.7 × 2.2 | ÷ 0.7 |
| DAU（选了才有） | × 3 | 不变 |
| RPD | Revenue_adj / Downloads_adj | 同左 |
| ARPDAU（选了才有） | Revenue_adj / DAU_adj | 同左 |

`÷0.7` 用于还原 Apple/Google 平台 30% 分成；CN 的 `×4 / ×2.2 / ×3` 是对 CN 区数据偏低的经验性上修。
要改这些系数：编辑 `scripts/fetch.py` 顶部的常量。

## 常见问题

- **报「未找到 token」/ 401 / 403**：没配 key、key 失效、或你的账号无该数据权限。回到「配置你的 API Key」，并确认 ST 订阅。
- **产品搜错了（撞名）**：先 `--resolve-only` 看匹配结果，再用更准确的名字，或直接在 `query.json` 里给 `unified_id`。
- **API 返回 0 条**：产品在所选区间无数据 / 国家码写错 / 日期格式不是 `YYYY-MM-DD`。
- **收入数量级看着不对**：脚本已把 API 返回的「美分」÷100 转成美元，口径只套一次，不要自己再换算。
- **缺依赖**：`pip install pandas numpy openpyxl requests`。

## 隐私与数据

- 数据来自 **Sensor Tower API**，用你自己的 key 调用，计入你自己的账号配额。
- 拉到的数据与生成的 Excel 都在你本地 / 你的环境，本 skill 不会把数据上传到任何第三方。
- 你的 API key 仅存于你的环境变量或本地 `config.local.json`（已 gitignore），不随 skill 分发。
