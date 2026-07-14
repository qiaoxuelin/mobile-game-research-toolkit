---
name: sensor-tower-fetch-byok
description: "BYOK (bring-your-own-key) version of the Sensor Tower API pull skill, each user must supply their OWN Sensor Tower API key (via ST_AUTH_TOKEN env var or a gitignored config.local.json). Pulls Downloads / Revenue (+ optional DAU) by product name + date range, applies a selectable caliber (net = ST native net revenue, no ÷0.7 [default]; or gross = ÷0.7 platform restore; CN uplifts default off; recomputed RPD/ARPDAU) — user confirms which caliber each run, outputs a 3-sheet Excel (明细 / 按App汇总 / 按地区汇总). Self-contained (does NOT need sensor-tower-data-adjust). Triggers: '自带 key 拉 ST 数据 / 用我自己的 key / BYOK sensor tower / fetch sensor tower with my own key / bring your own key sensor tower'."
---

# Sensor Tower API 一键取数（BYOK 自带 Key 版）

BYOK 版：**不内置任何 token**，每位使用者用自己的 Sensor Tower API key。给「产品 + 日期区间」即可拉**下载量 + 收入**（可选 DAU），
自动套可选口径（net=ST原始净收入不÷0.7 / gross=÷0.7还原；CN上修默认关；重算 RPD/ARPDAU），出 3-sheet Excel。不用手动导出 CSV。口径见 数据口径说明.md。


## 首次使用：配置你自己的 API Key（必做一次）

被触发后，**先确认 key 是否已配置**：跑一次 `--resolve-only`（见 Step 2），若脚本报
「未找到 Sensor Tower API token」，按下面任一方式配好再继续。三选一：

1. **环境变量（推荐，最不易误提交）**
   ```bash
   export ST_AUTH_TOKEN=<你的token>
   ```
2. **本地配置文件**（已 gitignore，不会被提交）：把 `config.local.example.json` 复制成 `config.local.json` 填入 key
   ```bash
   cp .claude/skills/sensor-tower-fetch-byok/config.local.example.json \
      .claude/skills/sensor-tower-fetch-byok/config.local.json
   # 然后编辑 config.local.json 的 "auth_token"
   ```
3. **直接改 `config.json` 的 `auth_token`** —— ⚠️ 该文件会被提交，**别把私钥推到公共仓库**，仅自用私库可。

取 token 优先级：环境变量 `ST_AUTH_TOKEN` > `config.local.json` > `config.json`。

> **去哪拿 key**：登录 Sensor Tower 账号 → 后台 API / Integrations 设置里生成 API token（需对应数据订阅权限）。
> 没有 ST 账号/订阅则无法使用本 skill（这是 Sensor Tower 的付费数据，BYOK 仅解决「用谁的 key」，不绕过订阅）。

## 安装与可用范围（云端 / 本地 / 全局）

skill 非云端专属，本地 CLI 一样能跑。两种安装范围：

| 范围 | 放哪 | 效果 |
|---|---|---|
| **仓库级** | 仓库内 `.claude/skills/sensor-tower-fetch-byok/` | 在该仓库目录开 Claude Code 即可用 |
| **用户级 / 全局** | 本地 `~/.claude/skills/sensor-tower-fetch-byok/` | 本地任意项目都能调 |

全局安装：`cp -r .claude/skills/sensor-tower-fetch-byok ~/.claude/skills/`（注意：**不要**把你的 `config.local.json` 一起拷到会被提交的地方）。
本地运行需 `pip install pandas numpy openpyxl requests` + 能联网到 `api.sensortower.com`。

## 口径表（由 query.json 的 `caliber` 字段选择，每次取数确认）

| 字段 | net（默认） | gross |
|---|---|---|
| `Downloads_adj` | `Downloads`（CN 上修默认关） | 同 net |
| `Revenue_adj ($)` | `Revenue`（ST 原始净收入，不 ÷0.7） | `Revenue / 0.7`（还原平台 30% 分成） |
| `DAU_adj`（选了 DAU 时） | `DAU`（CN 上修默认关） | 同 net |
| `RPD_adj` | `Revenue_adj / Downloads_adj` | 同左 |
| `ARPDAU_adj`（选了 DAU 时） | `Revenue_adj / DAU_adj` | 同左 |

- **net**：ST `sales_report_estimates` 返回值已是净收入（扣平台分成+税费），直接用，不 ÷0.7 —— 公司战略会分析默认。
- **gross**：`Revenue / 0.7` 还原成玩家毛支出，用于与 gross 口径外部数据对齐。两口径不混用。
- CN 区上修（×4/×2.2/×3）默认关；如需开启编辑 `scripts/fetch.py` 顶部 `CN_*_MULT` 常量。
- 口径详见 `数据口径说明.md`。

## 调用流程：确认 key → 先问 → 解析确认 → 一把拉完

### Step 0: 先问用户要什么（必做）

确认 key 已配置后，**先用 `AskUserQuestion` 把参数问清楚再动手**，至少确认：

1. **产品** —— 要哪些游戏/App（名字即可，可多个）。
2. **日期区间** —— `start_date` ~ `end_date`。
3. **口径** —— `net`（ST 原始净收入，不 ÷0.7，默认）还是 `gross`（÷0.7 还原毛支出）。**每次取数都让用户确认选哪种**，不替用户决定。

用户没特别说的用 config 默认并讲明：国家默认 `["CN","US","JP","KR","OTHER"]`（"OTHER" = WW 减其余四区）、
os 默认 `unified`、粒度默认 `monthly`、DAU 默认否。

### Step 1: 写 query.json

```json
{
  "products": ["Genshin Impact", {"name": "鸣潮", "unified_id": "<unified_app_id>"}],
  "countries": ["CN", "US", "JP", "KR", "OTHER"],
  "start_date": "2024-01-01",
  "end_date": "2024-12-31",
  "granularity": "monthly",
  "os": "unified",
  "include_dau": false,
  "caliber": "net"
}
```

- `products` 每项可为字符串（按名搜索）或 `{"name","unified_id"}`（给 id 跳过搜索、更稳）。
- `countries` 写 `"OTHER"` 触发「其它地区 = WW − 其余国家」推导。
- `caliber` 收入口径：`"net"`（默认，ST 原始净收入不 ÷0.7）或 `"gross"`（÷0.7 还原毛支出）。每次取数由用户确认。
- 必填：`products / start_date / end_date`；其余缺省取 config 默认。

### Step 2: 先 `--resolve-only` 确认匹配（也用来检查 key）

```bash
python3 .claude/skills/sensor-tower-fetch-byok/scripts/fetch.py <query.json> --resolve-only
```

- 若报「未找到 token」→ 回到「首次使用」配 key。
- 打印每个产品匹配到的 名称 / 发行商 / 上线日期 / unified_id + 候选；撞名就用准确名或 `unified_id` 重跑。

### Step 3: 正式拉数 + 调整 + 出 Excel

```bash
python3 .claude/skills/sensor-tower-fetch-byok/scripts/fetch.py <query.json> [<output.xlsx>]
```

输出默认在 query.json 同目录、文件名加 `_ST_adjusted.xlsx`。3 张 sheet：明细 / 按App汇总 / 按地区汇总。

### Step 4: 核对 QA + 交付

检查产品数/国家/区间一致；确认输出 Excel 用的口径（net 时 `Revenue_adj=原始 Revenue`；gross 时 `Revenue_adj=原始/0.7`，即 `Revenue_adj/原始≈1.4286`）；CN 上修默认关故 CN 行无放大；
选了 DAU 时确认 ARPDAU 已生成（Other 行 DAU 留空）。用 `SendUserFile` 把 Excel 发给用户并给关键洞察，**注明本次口径是 net 还是 gross**。

## 常见排查

- **未找到 token / 401 / 403** → 没配 key 或 key 无效/无该数据权限。按「首次使用」配自己的 key；ST 数据需对应订阅。
- **产品搜错（撞名）** → `--resolve-only` 看匹配，再用准确名或 `unified_id`。
- **API 返回 0 条** → 产品在所选区间无数据 / 国家码错 / 日期非 `YYYY-MM-DD`。
- **收入数量级不对** → 脚本已把「美分」÷100 转美元；确认 caliber 是否选对（net 不 ÷0.7、gross ÷0.7，两者差 ~1.43 倍），别自己额外换算。
- **依赖缺失** → `pip install pandas numpy openpyxl requests`。

## 数据口径备注

- 销售只用 `unified/sales_report_estimates` 一个端点（自带 iPhone/iPad/Android 拆分，按 os 选字段）；收入返回「美分」已 ÷100。
- DAU 取 `unified/usage/active_users`（day 粒度），按 period 取月内日均。内置退避重试（2s/4s/8s）。

## 本 skill 不做的事

- 不内置任何 token（这正是 BYOK 的目的）；不绕过 Sensor Tower 订阅。
- 不处理手动 CSV 导出（用 `sensor-tower-data-adjust`）。
- 不替用户决定口径（net/gross 每次由用户确认；CN 上修系数改 `fetch.py` 顶部常量）。
