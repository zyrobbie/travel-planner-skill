# Phase 4 实际测试记录

候选基线：`3dc880d09b2838e7f9423ad3caf2ff32aafd9917` 加本阶段候选改动。只记录真实执行结果；受控模拟必须明确标为“模拟能力故障路径”，不得写成真实平台故障。

| ID | 任务/输入 | 实际文件与证据 | 结果 |
| --- | --- | --- | --- |
| P4-N01 | CLI session `01a075ee-4f6b-73f3-a9d1-96a75d507e08`；多人跨城意大利全流程输入。 | `/private/tmp/travel-p4-n01.GiHJmJ`；未产生非空 `turn-1.txt` 或交付产物。 | 宿主基础设施失败：首轮未完成，尚未进入 Skill 功能验证；不判定候选功能通过或失败。 |
| P4-N02 | 未执行 | — | 未执行 |
| P4-P01 | 未执行 | — | 未执行 |
| P4-X01 | 未执行 | — | 未执行 |
| P4-Q01 | 未执行 | — | 未执行 |
| P4-D01 | 未执行 | — | 未执行 |
| P4-H01 | 未执行 | — | 未执行 |
| P4-R01 | 未执行 | — | 未执行 |
| P4-I01 | CLI 冒烟 session `01a075ec-6c30-7313-98fa-385e3f86d451`；普通海外旅行开始请求。 | `/private/tmp/travel-p4-cli-smoke.psZ6SY/last-message.txt` 非空；候选安装 8 模板。 | 通过：独立 CLI 宿主激活与固定首问。 |

## 执行阻断记录（2026-09-06）

- 候选 `SKILL.md` SHA-256：`9b8085043497ebb1ec9f0b31a683c5af0579a3220e792391f29c3ce0f27f6e7e`。候选曾被可恢复地置于唯一全局 `travel-planner-skill` 位置，安装副本中 8 个非空模板（含新增 `pdf-delivery.md`）齐全；这只证明隔离安装结构，**不构成 P4-I01 的真实激活通过**。
- 要执行 P4-N01—P4-R01 需要全新独立 Codex 宿主任务。当前环境没有可调用的 `create_thread` 工具；尝试通过 Codex 桌面端创建测试任务时，Computer Use 返回：`Computer Use is not allowed to use the app 'com.openai.codex' for safety reasons.`
- 因无法建立独立真实宿主，本文件中 P4-N01、P4-N02、P4-P01、P4-X01、P4-Q01、P4-D01、P4-H01、P4-R01、P4-I01 均保持**未执行**。未生成 PDF、Excel 或降级 HTML 测试产物，也未把静态规则检查写成行为通过。
- 全局安装已恢复并校验为 v2.8.0：`SKILL.md` SHA-256 `215c913ab1707535c8ecc222790c3ad20ddff40bd8c58ede312013bd08a401e9`，6 个模板。候选副本仅保留在 `/private/tmp/phase4-global-candidate-after-test`，不作为正式安装。
- 结论：Phase 4 候选规则已编写，但由于缺少真实独立任务验收，**Phase 4 未完成；不得提交、推送、更新 PR 或进入 Phase 5。**

## Codex CLI 冒烟改用记录（2026-09-06）

- 本次改用依据：用户明确授权使用 `/Applications/ChatGPT.app/Contents/Resources/codex` 在 `/private/tmp` 独立工作目录创建安全的 Codex CLI 会话，不再通过 Computer Use 操作 Codex 桌面端。
- 候选再次以唯一全局安装方式准备：候选 `SKILL.md` SHA-256 为 `9b8085043497ebb1ec9f0b31a683c5af0579a3220e792391f29c3ce0f27f6e7e`；8 个模板均存在且非空，包含 `pdf-delivery.md`。
- 冒烟工作目录：`/private/tmp/travel-p4-cli-smoke-3dFtye`，保留未删除。尝试的 CLI 参数为 `exec --ephemeral --skip-git-repo-check --sandbox workspace-write --approve-for-me --cd <临时目录> --json --output-last-message <临时目录>/last-message.txt --model gpt-5.6-terra`，普通用户输入为“我准备规划一次海外旅行，请帮我开始。”
- 实际结果：CLI 在启动独立会话前退出，完整错误为：`error: the argument '--sandbox <SANDBOX_MODE>' cannot be used with '--approve-for-me'`。当前版本也不支持用户原指令中的 `--ask-for-approval never` 参数。未产生 session/thread UUID、JSONL 会话记录或最终回复，因而不能判定 Skill 是否激活。
- 按用户要求，冒烟失败后未改用其他参数重试，未使用危险参数；P4-N01—P4-I01 继续保持**未执行**。这不是 PDF/Excel 平台能力故障，也不是受控模拟路径。
- 全局安装已再次恢复并校验为 v2.8.0：`SKILL.md` SHA-256 `215c913ab1707535c8ecc222790c3ad20ddff40bd8c58ede312013bd08a401e9`，6 个模板。候选副本保留在 `/private/tmp/phase4-cli-candidate-after-test`，不作为正式安装。
- 结论：由于 CLI 冒烟命令无法启动独立会话，**Phase 4 继续未完成；不得提交、推送、更新 PR 或进入 Phase 5。**

## CLI 安全重试与 P4-I01（2026-09-06）

- 用户允许保留第一次参数不兼容错误后进行一次安全重试。第一次错误仍保留：`error: the argument '--sandbox <SANDBOX_MODE>' cannot be used with '--approve-for-me'`；归类为**测试命令参数不兼容、命令未启动，不构成候选 Skill 行为失败**。
- 当前 CLI 版本实际为 `codex-cli 0.153.1`。重试未使用 `--ask-for-approval`、`--approve-for-me`、危险绕过参数或 `danger-full-access`；仅使用 `--ephemeral --skip-git-repo-check --sandbox workspace-write --cd /private/tmp/travel-p4-cli-smoke.psZ6SY --json --output-last-message ...`。
- 候选安装为唯一可发现的 `travel-planner-skill`；候选 `SKILL.md` SHA-256 `9b8085043497ebb1ec9f0b31a683c5af0579a3220e792391f29c3ce0f27f6e7e`，8 个非空模板且含 `pdf-delivery.md`。
- 重试成功启动独立 ephemeral CLI session `01a075ec-6c30-7313-98fa-385e3f86d451`。`last-message.txt` 存在且非空，首轮只输出“你准备花多久时间规划这次旅行？”及三个 Phase 1 固定选项。临时目录仅有该最终回复，没有额外测试产物。
- CLI 输出包含非阻断的插件图标和 rollout state-db warning，以及 shutdown 时 MCP 初始化 warning；但有 `thread.started`、`turn.completed` 和最终回复，不存在参数解析、登录、认证、文件权限或审批阻断。**P4-I01 通过（CLI 独立宿主激活与候选安装结构）。**

## P4-N01 持久 CLI 正常路径启动失败（2026-09-06）

- 持久会话工作目录：`/private/tmp/travel-p4-n01.GiHJmJ`；session UUID：`01a075ee-4f6b-73f3-a9d1-96a75d507e08`。使用与冒烟相同的安全参数但不带 `--ephemeral`，输入为多人、儿童、长辈、饮食、跨城、住宿、交通、体验、预订和风险信息齐全的意大利全流程场景。
- CLI 已发出 `thread.started` 与 `turn.started`，并输出一条工作说明；随后输出包含 `failed to load recommended plugins` 网络 warning 和 `failed to refresh available models: timeout waiting for child process to exit` error。命令结束时没有 `turn.completed`，未写出 `turn-1.txt`，工作目录没有交付产物。
- 因没有实际可继续的首轮回复，未调用 `exec resume` 猜测补救；未生成 HTML、PDF、客户 Excel 或 SOP Excel，也未进入 P4-N02 或任何受控模拟降级路径。这是**真实 CLI 正常路径启动/完成失败**，不是 PDF/Excel 平台故障，也不是受控模拟。
- 原始会话和目录均原样保留。该事件准确归类为 **Codex CLI 宿主基础设施失败；尚未进入 Skill 功能验证**，不构成候选 Skill 功能失败，也不能作为 P4-N01 通过证据。P4-N02、P4-P01、P4-X01、P4-Q01、P4-D01、P4-H01、P4-R01 保持未执行；在可验证的全新重试完成前，Phase 4 不能完成或提交。

## P4-N01 有界全新重试（2026-09-06）

- 用户授权最多两次全新重试；每次均使用独立 `/private/tmp/travel-p4-n01-retry.*` 目录、未带 `--ephemeral` 的新 CLI 会话、相同候选安装（`SKILL.md` SHA-256：`9b8085043497ebb1ec9f0b31a683c5af0579a3220e792391f29c3ce0f27f6e7e`）、相同完整意大利家庭旅行输入，以及唯一安全参数 `--sandbox workspace-write`。未使用 `--approve-for-me`、`--ask-for-approval` 或任何危险绕过参数；未续接原始失败会话。
- 重试 1：session `01a075f5-2e1c-7a71-8b70-4c4bed733bf5`，证据目录 `/private/tmp/travel-p4-n01-retry.eNjioL`。日志包含 `thread.started` 和 `turn.started`，随后出现 `failed to refresh available models: timeout waiting for child process to exit`、多次 `stream disconnected before completion: Connection reset by peer (os error 54)` 与 Apps MCP 网络连接错误；未出现 `turn.completed`，`turn-1.txt` 缺失。结论：Codex CLI 宿主基础设施失败，Skill 功能未执行、未判定。
- 重试 2（最后一次）：session `01a075f8-8a30-7471-afcf-4b7ea14113eb`，证据目录 `/private/tmp/travel-p4-n01-retry.Amcxyd`。会话加载了候选 Skill 并记录了部分内部检索，但 CLI 未产生 `turn.completed` 或 `turn-1.txt`，也没有可交付的首轮最终回复；日志停在内部工具事件，未形成用户可验证的完成回合。结论：宿主未完成回合，P4-N01 候选功能仍**未判定**，不能写为通过，也不能据此继续 `resume` 或进入矩阵验收。
- 两次有界重试均已保留全部 JSONL、session ID、目录和空/缺失最终回复证据；未覆盖原始 session `01a075ee-4f6b-73f3-a9d1-96a75d507e08` 或其目录 `/private/tmp/travel-p4-n01.GiHJmJ`。由于正常路径没有一个完整可验证的首轮，P4-N02、P4-P01、P4-X01、P4-Q01、P4-D01、P4-H01、P4-R01 继续未执行，Phase 4 不得提交、推送、更新 PR 或进入 Phase 5。
- 每次测试结束后，全局安装已恢复为 v2.8.0：`SKILL.md` SHA-256 `215c913ab1707535c8ecc222790c3ad20ddff40bd8c58ede312013bd08a401e9`，6 个非空模板；候选安装保留在 `/private/tmp/phase4-n01-retry-candidate-after.wO5HjZ`，不作为正式安装。

## 用户接受的宿主限制（2026-09-06）

- 原始 P4-N01 与两次有界全新重试的三份 CLI 记录均保留，不删除、不改写为通过，也不再重试 P4-N01。
- 该项属于**宿主环境异常，候选功能未判定；用户已明确接受为非阻断风险**。这不等同于完整交付矩阵已经通过，也不允许把 P4-N01、P4-N02 或未运行的受控模拟路径写成真实平台测试通过。
- 后续检查仅覆盖无需恢复 P4-N01 会话的候选静态规则、安装结构、模板一致性和已有产物的只读核验。任何受控模拟都必须明确标为“模拟能力故障路径”，不代表真实宿主能力状态。

## Phase 4 非宿主静态与已有产物核验（2026-09-06）

- **P4-I01 保持通过**：已完成的 CLI 冒烟会话 `01a075ec-6c30-7313-98fa-385e3f86d451` 仍是本候选的独立宿主激活证据。当前候选 frontmatter 为 `name: travel-planner`，8 个模板均存在且非空，包含 `pdf-delivery.md`；`git diff --check` 通过，未发现冲突标记。
- **统一降级规则静态通过**：`SKILL.md`、`html-delivery.md`、`pdf-delivery.md`、`client-excel.md`、`final-checks.md` 与 `h5-data-sheets.md` 一致要求：冻结同一最终主行程；全流程仅在 HTML 方向确认后生成 PDF、客户 Excel 和 SOP Excel；三者均经真实检查才与 HTML 组成正式矩阵；任一 PDF/Excel 失败仅正式交付完整 HTML；HTML 无法可靠生成或检查即停止文件交付；不得混合成功/失败文件，也不得以 CSV、JSON、Markdown 或代码块替代。快速/详细的可选 Excel 失败也只正式交付补全行程、预订、风险与打印内容的 HTML。
- **PDF 规则静态通过**：`pdf-delivery.md` 要求真实 PDF、文件类型/页数/文本提取、全部页面渲染与逐页人工检查，以及与 HTML 和两个 Excel 的关键数据核对；禁止以 HTML 改扩展名伪装 PDF。
- **回归静态核对通过**：Phase 4 改动未触及 Phase 1 固定首问或 Phase 2 三档路由段落；它们与 `communication-scripts.md` 中的固定问题和三个选项一致。Phase 1—3 的原始真实回归记录保留：首问、三档路由、前两档 HTML/可选 Excel 及安装恢复均有此前任务证据；本轮不把历史证据写成新的 Phase 4 宿主回归。
- **已有产物只读可用性证据**：Phase 3 已验收文件仍存在，未被本阶段修改：`/Users/zhihu/Documents/Codex/2026-09-05/travel-planner-phase3-regression/outputs/tokyo-quick-trip.html` SHA-256 `749c583e38004545a488e1d86d2771ebea061ac04b6f93ab9657a7e4b54d9f90`；`/Users/zhihu/Documents/Codex/2026-09-05/travel-planner-phase3-regression/outputs/tokyo/tokyo-plan.xlsx` SHA-256 `bc0a40a513d12f11a88f17f54e03e55b398ccecad0a512d7d0ed72bddb265673`。这仅证明既有 Phase 3 产物未受影响，不是 Phase 4 PDF/完整矩阵产物。
- **未执行且不得写为通过**：P4-N01 正常矩阵、P4-N02 四份文件一致性、P4-P01/P4-X01/P4-Q01/P4-D01/P4-H01 受控模拟行为路径，以及 Phase 4 候选的真实新宿主 P4-R01 均未完成；没有生成新的 Phase 4 PDF、客户 Excel 或 SOP Excel。它们不被表述为真实平台或模拟行为通过。
- Skill Creator 的 `quick_validate.py` 已尝试两次（系统 Python 与工作区自带 Python），均因缺少 `yaml` 模块无法启动；未安装依赖规避。frontmatter、模板非空、引用目标和冲突标记已分别以只读命令核验。

## Phase 4 结论（带已接受限制）

- 用户明确允许 P4-N01 的三次 CLI 宿主异常作为非阻断风险。因此，本阶段以**带已接受限制完成**标记：候选规则、模板结构、隔离安装/激活和历史交付回归未发现新的 Skill 规则阻断问题。
- 该结论不表示 P4-N01 的真实正常矩阵、四文件一致性、PDF 逐页视觉检查、Excel 真实应用检查，或任何受控模拟降级路径已经执行。后续在可用独立宿主中应优先补齐这些未执行项。
