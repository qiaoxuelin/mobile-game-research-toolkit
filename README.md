# Mobile Game Research Toolkit

一套海外手游市场数据研究的 Claude Code agent skills，覆盖**研究方法论 -> 数据取数 -> 作图 -> 飞书文档交付**的完整流程。4 个 skill 可独立使用，也可串成一条从"判断一个赛道/趋势"到"飞书专题报告交付"的工作链。

## Skills

| skill | 作用 | 触发 |
|---|---|---|
| [`mobile-game-strategy-research`](skills/mobile-game-strategy-research/) | 研究方法论：先判断再展开、不做泛泛综述、数据+产品拆解结合。输出飞书长报告（专题报告/选题跟踪），单品拆解可用 9 字段模板。含赛道/趋势研究框架示例。 | 手游战略研究 / 研究一个手游产品 / 判断一个市场趋势 / 赛道研究 / 产品案例拆解 |
| [`sensor-tower-fetch-byok`](skills/sensor-tower-fetch-byok/) | Sensor Tower 数据取数（BYOK，自带 token）。下载/收入/活跃/排名/买量素材等。net/gross 口径每次由用户确认。 | 拉 ST 数据 / Sensor Tower / 下载收入趋势 |
| [`chart-style`](skills/chart-style/) | 咨询风格图表（insight headline / minimal ink / direct labels / source line），产出 inline SVG 或 slide-ready PNG。含飞书 whiteboard 适配作图规范。 | 作图 / visualize / chart / slide visual / waterfall / scatter |
| [`feishu-doc-editing`](skills/feishu-doc-editing/) | 飞书云文档（lark-cli）编辑：fetch -> 定位 block_id -> block_insert_after/block_replace/str_replace -> 验证；whiteboard SVG 图表；评论读取；classifier 阻塞 fallback。汇总 6 条核心踩坑。 | 编辑飞书文档 / block_insert_after / block_replace / str_replace / 飞书图表 / 飞书评论 |

## 流程链

```
研究问题
  └─ mobile-game-strategy-research  (判断+框架+产出结构)
       ├─ sensor-tower-fetch-byok   (拉数据，确认 net/gross 口径)
       ├─ chart-style                (数据画成咨询风格图)
       └─ feishu-doc-editing         (写入飞书专题报告/选题跟踪，交付)
```

飞书是交付面（专题报告/选题跟踪在飞书维护），本地只存过程与快照。

## 安装

把 `skills/` 下需要的 skill 文件夹拷到 Claude Code 的 skills 目录：

- 仓库级：仓库内 `.claude/skills/<skill-name>/`
- 用户级/全局：`~/.claude/skills/<skill-name>/`

```bash
# 示例：装全部到用户级
cp -r skills/* ~/.claude/skills/
```

## 配置 / 外部依赖

| skill | 外部依赖 | 安装 / 配置 |
|---|---|---|
| `sensor-tower-fetch-byok` | Sensor Tower API（付费，BYOK）+ Python 3 | `pip install pandas numpy openpyxl requests`；用 `ST_AUTH_TOKEN` 环境变量或 `config.local.json`（已 gitignore）填你自己的 ST token，见 [`config.local.example.json`](skills/sensor-tower-fetch-byok/config.local.example.json)。口径见 [`数据口径说明.md`](skills/sensor-tower-fetch-byok/数据口径说明.md) |
| `feishu-doc-editing` | `@larksuite/cli`（飞书官方 CLI，公开 npm 包） | `npm i -g @larksuite/cli`（提供 `lark-cli` 命令）；`lark-cli auth` 完成飞书 OAuth 授权。⚠ **不是** `npm i -g lark-cli`（那是另一个不相关的 0.1.0 包） |
| `chart-style` | 无（产出 SVG，语言不限） | 可选 Python 生成 SVG |
| `mobile-game-strategy-research` | 无 | - |

## 备注

- `mobile-game-strategy-research/frameworks/` 下两个框架示例（肉鸽赛道 / 市场新趋势）是**占位示例**，展示如何为某研究方向搭框架；其中的假设/产品/数据是占位，请替换为你自己的研究或删除。
- 模板里的 CSV 示例行同样是占位，替换为你的真实案例或删除。

## License

MIT，见 [LICENSE](LICENSE)。
