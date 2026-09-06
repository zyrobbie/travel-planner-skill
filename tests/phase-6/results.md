# Phase 6-A 最终发布候选测试记录

候选基线：`6c1a1368116ac029c83d2ad9656ef0665ff97276`，`main` 与 `origin/main` 均为 `44b8160efd590b05488d794df66f7ec711d65cb7`。本文件区分本轮真实执行、历史证据复用、静态检查、未执行与用户已接受限制。

| ID | 分类 | 实际证据 | 结果 |
| --- | --- | --- | --- |
| P6-E01 | 真实 CLI | session `01a076ee-9d1c-7b22-beb7-17f277461a9a`，`/private/tmp/travel-p6-entry.cWlppM`。普通请求首轮只输出固定问题和三个逐字选项。 | 通过 |
| P6-Q01 | 真实 CLI | session `01a076ef-9795-70f2-91de-35cd3f79982e`，`/private/tmp/travel-p6-quick.J1rhHu`。已明确快速档后直接给巴黎 3 晚建议、预算口径、住宿、交通与每日安排，未重复问档位或进入阶段式流程；最终回复实际完成。 | 通过 |
| P6-D01 | 真实 CLI | session `01a076f1-7efd-7521-9784-ee4a5bd10025`，`/private/tmp/travel-p6-detail.7dcKWl`。明确“收齐基本条件 → 确认路线方向 → 做出每天的详细安排”，集中收集条件与风险确认，不退回冗长阶段确认。 | 通过 |
| P6-F01 | 真实 CLI | session `01a076f2-52f7-7b72-852a-379196e20986`，`/private/tmp/travel-p6-full.w5pVXd`。保留两家庭、儿童、长辈步行、素食、预算、房间、住宿、交通、签证与 2027 风险复核要求。 | 通过 |
| P6-S01 | 真实 CLI + 静态 | session `01a076f3-5167-7480-80c8-a08a4b3dce85`，`/private/tmp/travel-p6-safety.YZhCLN`。回复“我不能代你下单或付款”，并仅提供订单核对与官方入口；`SKILL.md` 和 `communication-scripts.md` 共同保留无条件禁止采购、支付资料和申请提交规则。 | 通过 |
| P6-I01 | 真实 GitHub 安装 | 使用 Skill Installer 从 `zyrobbie/travel-planner-skill` 的 `codex/v3.0.0-dev` 安装到 `/private/tmp/travel-p6-install.iEE1w3/travel-planner-skill`。一次安装包含 `SKILL.md`、`README.md`、`LICENSE` 与 8 个非空模板；安装副本与候选 `SKILL.md` SHA-256 均为 `9b8085043497ebb1ec9f0b31a683c5af0579a3220e792391f29c3ce0f27f6e7e`。 | 通过 |
| P6-R01 | 静态/审查 | `git diff --check` 通过；`v2.8.0...HEAD` 只包含 Skill、模板、README 与文字测试记录；8 个模板均非空、引用目标存在、无冲突标记、未发现运行规则引用仓库外路径。README 现以根 GitHub 地址作为正式安装入口，开发分支仅用于贡献者或发布候选验证。 | 通过，见 Git fsck 限制 |

## 历史证据复用

- Phase 0：2.8.0 基线、真实宿主激活、H5 与 Microsoft Excel for Mac 检查记录保留在 `tests/phase-0/`。
- Phase 1—3：首次档位选择、三档路由、采购规则、HTML 与可选 Excel 的真实任务证据保留在 `tests/phase-1/`、`tests/phase-2/`、`tests/phase-3/`。
- Phase 4：完整矩阵和统一 HTML 降级规则已实现并静态核对；其正常矩阵、PDF/Excel 真实应用、降级模拟未完整执行的事实保留在 `tests/phase-4/results.md`。
- Phase 5：一次完整 GitHub 安装和三档最小冒烟已完成，记录保留在 `tests/phase-5/`。

## 未执行与用户已接受限制

- 本轮未重跑完整 HTML/PDF/Excel 矩阵、PDF 逐页检查、Excel 全平台检查或受控降级模拟；这不是通过结论。
- Phase 4 的三次 P4-N01 CLI 宿主异常及完整矩阵限制仍为用户接受的非阻断风险；不改写为通过。
- 本轮每个 CLI 会话启动时出现 `rollout state-db discrepancy` warning，但五个最终回复均完成；不据此宣称所有宿主、模型或网络环境无偶发问题。
- 未验证 Claude Code、Cursor、WorkBuddy、Windows、Linux、WPS、Numbers 或所有 PDF/表格应用；README 不宣称这些平台已通过。

## Git fsck

- `git fsck --no-reflogs` 报告一个本地不可达旧提交：`28a0e811946b0a455c848fd686a31ecdac7fbb1b`（2026-09-04 的 Phase 0 测试记录提交）。它不在 `main`、`origin/main`、开发分支或 PR 历史中，本轮未删除、重写或用清理命令处理；作为本地仓库卫生提示保留，不影响当前发布候选提交链。

## Phase 6-A 结论

- **完成。** 本轮未发现安装失败、固定路由错误、主要能力丢失、采购许可或 README/实现冲突等新的关键阻断问题。
- 发布候选可进入人工 PR 审查；本结论不授权合并、推送 main、创建 `v3.0.0` 标签、创建 Release 或合并后冒烟。
