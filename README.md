travel-planner · 海外旅行规划师
English: An agent skill that turns any AI agent into a professional overseas travel planner. Instead of dumping a generic itinerary on the first reply, it walks users through phased planning — constraints → group consensus → route skeleton → experience priorities → daily rhythm → key bookings → risk audit → final delivery — and produces executable travel plans with three standard deliverables (a 3-minute client Excel, a direction-confirmation H5 page, and a maintainable SOP archive). To install, just tell your agent: "Install this skill: https://github.com/zyrobbie/travel-planner-skill"

一个面向 AI Agent 的海外旅行规划方法论 Skill。不堆砌景点清单，而是通过分阶段沟通，帮用户做出真正可执行的旅行方案。

安装 / Install
方式一：一句 Prompt 安装（推荐）

直接对你的 Agent 说：

帮我安装这个 skill：https://github.com/zyrobbie/travel-planner-skill

支持 Agent Skills 规范的工具（WorkBuddy、Claude Code、Cursor 等）会自动拉取仓库，把 SKILL.md 和 templates/ 装入本地 skills 目录，之后提出海外旅行规划需求时自动激活。

方式二：手动安装

git clone https://github.com/zyrobbie/travel-planner-skill.git
然后把 SKILL.md 和 templates/ 复制到你的 Agent 工具的 skills 目录，例如 ~/.workbuddy/skills/travel-planner/ 或 ~/.claude/skills/travel-planner/。

方式三：零安装试用

把 SKILL.md 全文粘贴进任意 LLM 对话作为系统提示词，直接开始规划。

这是什么
travel-planner 定义了 AI 扮演海外旅行规划师时的完整工作方法：

分阶段收敛（阶段 0—8）：阶段导航 → 任务定义与旅行约束 → 团队共识与旅行治理（可选）→ 路线骨架 → 体验偏好与取舍 → 一日一行日程草案 → 关键预订与出发准备（A/B/C 三级）→ 行程核对与风险审计 → 定稿确认与版本冻结。每个阶段只解决一个层级的问题，用户确认后才进入下一阶段
四角色一体：需求分析师、路线规划师、风险审计员、交付设计师
事实核验纪律：信息六级状态管理（已确认/待确认/待补/方向待定/备选/不适用）；农历节日、宗教节日等日期事实必须联网核验，不得凭记忆推算；高风险信息以官方来源为准
三份标准交付物：用户简明版 Excel（3 分钟看懂）、轻量方向确认 H5、SOP 沟通档案 Excel（9 张阶段 Sheet + 6 张 H5 数据接口，可供任何规划师或 Agent 接续维护）
沟通规范：结构化选项必须带说明、小众名称必须「中文可识别名（英文/当地名）」+ 一句话解释、内部术语不外泄（人话原则）
文件生成安全规范：UTF-8 显式声明、U+FFFD 乱码防御与回退方案
目录结构
travel-planner-skill/
├── SKILL.md                          # 完整 SOP 正文（frontmatter + 方法论）
└── templates/                        # 标准交付物模板
    ├── stage-sheets.md               # SOP 沟通档案：阶段 0—8 共 9 张 Sheet 结构
    ├── client-excel.md               # 用户简明版 Excel：3 张 Sheet
    ├── h5-data-sheets.md             # H5 数据接口：6 张工作表（字段顺序固定）
    ├── h5-page-spec.md               # 方向确认 H5：页面结构与手机适配校验清单
    ├── communication-scripts.md      # 沟通话术：人话原则、开场白、选项规范、命名规则
    └── final-checks.md               # 一致性检查、id 关联校验、最终验收 12 条
设计哲学
一份攻略达到以下条件才算真正完成：每段跨城交通真实存在且可执行；每天有明确主体验；多人移动、吃饭和集合时间已计入；高风险信息有官方依据；每天都有可删除项或备选方案；出行者当天不需要重新做重大决定。

旅行攻略的最终目标，不是把尽可能多的景点装进行程，而是让同行者清楚知道：今天为什么这样安排、下一步去哪里、什么必须完成、什么可以放弃，以及发生变化时怎么办。

License
MIT © 2026 zyrobbie
