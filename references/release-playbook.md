# Sloth Eido 技能发版规范与质量守门机制 V1.0

> 适用于 Sloth-Eido 系列所有 Agent Skills 的版本管理、测试验证和发布流程。
> 目标：让每个技能在发版时都经过统一的质量检查，避免"边上线边修"的被动局面。

---

## 一、目录结构规范

每个 Sloth 技能仓库必须包含以下文件和目录：

```
sloth-xxx-eido/
├── SKILL.md              # 核心技能定义文件（必需）
├── README.md             # 中文说明文档（必需）
├── README_EN.md          # 英文说明文档（推荐）
├── CHANGELOG.md          # 版本变更日志（必需，从 v1.0.0 开始维护）
├── LICENSE               # 开源协议（推荐 MIT）
├── .gitignore            # Git 忽略规则
├── references/           # 知识库和参考模板（推荐）
│   ├── *.md              # 各模块详细模板文件
│   └── regression-test-suite.md  # 回归测试套件（v1.1+ 推荐）
├── templates/            # CSV/数据模板（如 CPQ 技能）
├── scripts/              # Python 辅助脚本（如需要）
└── assets/               # 图片、头像、二维码等静态资源
```

**约束**：
- `SKILL.md` 是技能入口，名称不允许变更。
- `references/` 目录下的文件必须在 `SKILL.md` 中被引用，不允许存在孤立的参考文件。
- 所有数据文件（CSV、JSON、SQLite）必须有 `last_updated` 字段或版本标记。

---

## 二、版本号管理规范

采用 [Semantic Versioning 2.0.0](https://semver.org/lang/zh-CN/) 标准：

```
主版本号.次版本号.修订号
X.Y.Z
```

**升级规则**：

| 升级类型 | 场景 | 示例 |
|----------|------|------|
| **Z+1（修订号）** | Bug 修复、文案优化、标注修正，不改变功能行为 | v1.1.2 -> v1.1.3 |
| **Y+1（次版本号）** | 新增功能模块、新增输出格式、增强现有模块能力、新增测试套件 | v1.1.3 -> v1.2.0 |
| **X+1（主版本号）** | 破坏性变更（模块移除、意图路由规则重写、输出结构大幅调整）、架构级重构 | v1.2.0 -> v2.0.0 |

**版本号同步**：
- `SKILL.md` 头部的 `version:` 字段必须与 `CHANGELOG.md` 中的最新版本一致。
- Git tag 建议使用 `v1.2.0` 格式，与 CHANGELOG 标题对齐。

---

## 三、CHANGELOG 格式规范

基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，强制使用以下结构：

```markdown
# Changelog

## [1.2.0] - 2026-04-21

### Added
- 新增功能描述，一句话说清楚"加了什么"和"解决了什么问题"。

### Changed
- 对现有功能的改动描述。

### Fixed
- Bug 修复描述，关联 issue 编号（如有）。

### Deprecated
- 即将移除的功能预警。

### Removed
- 已移除的功能。

## [1.1.0] - 2026-04-19
...

## [1.0.0] - 2026-04-15

### Added
- Initial release.
- 核心功能列表。
```

**规则**：
- 每次发版必须有对应的 CHANGELOG 条目，不允许"空发版"。
- `Added/Changed/Fixed` 至少出现一项，不允许所有分类为空。
- 日期格式统一为 `YYYY-MM-DD`。
- 中英文 CHANGELOG 内容一致，优先维护中文版本。

---

## 四、发版检查清单（Release Checklist）

任何技能在从 `main` 分支发版前，必须完成以下检查。由技能维护者勾选确认。

### 4.1 代码与文档检查

- [ ] `SKILL.md` 中 `version:` 已更新至目标版本号
- [ ] `CHANGELOG.md` 已新增目标版本的变更条目
- [ ] `README.md` 中的功能描述与当前版本能力一致
- [ ] `references/` 目录下无孤立文件（所有文件在 `SKILL.md` 中被引用）
- [ ] 无临时文件、无敏感信息（API Key、内部密码、客户真实数据）泄露
- [ ] `.gitignore` 已排除不应提交的文件（如 `.env`、`*.db`、`__pycache__`）

### 4.2 功能检查

- [ ] 所有模块的意图路由触发词正常工作
- [ ] 输入不足时的追问/降级逻辑正常
- [ ] 闪电模式、标准模式、深潜模式三种密度适配正常
- [ ] 跨模块上下文传递路径已验证（至少验证 A->C、A->F、C->E 三条核心链）
- [ ] 去 AI 味规则自查通过（全文无禁用词汇）
- [ ] 标注规范自查通过（使用标准标签，无自创变体）

### 4.3 测试检查

- [ ] 单模块测试用例全部通过（参考 `references/regression-test-suite.md`）
- [ ] 跨模块工作链测试至少通过 3 条（A->C->E->F 为必选）
- [ ] 通用规则检查全部通过（标注规范、去 AI 味、密度适配、意图路由）
- [ ] 新功能对应的测试用例已补充到回归测试套件

### 4.4 提交与发布

- [ ] Git commit message 遵循规范（见第六节）
- [ ] 已推送至 GitHub `main` 分支
- [ ] 已创建 Git tag（格式 `vX.Y.Z`）
- [ ] 已向用户/QoderWork 提交更新（如适用）

---

## 五、测试报告模板

每次重大版本发版（Y+1 或 X+1），必须生成测试报告并存入 `references/test-report-vX.Y.md`。

### 报告结构

```markdown
# [技能名] 测试报告 VX.Y.Z

**测试日期**：YYYY-MM-DD
**测试场景**：（一句话描述测试场景）
**测试执行**：（执行人/Agent）
**技能版本**：VX.Y.Z

## 一、测试概览
（表格：模块、测试场景、数据置信度）

## 二、各模块测试结果明细
（每个模块的验证项表格：验证项、结果、说明）

## 三、跨模块能力验证
（上下文传递路径的验证状态）

## 四、综合评分
（功能完整性评分 + 数据置信度总评）

## 五、改进优化建议
（按优先级分类：高/中/低）

## 六、结论
（是否具备生产环境使用条件，推荐状态）
```

**规则**：
- 测试报告不是"一次性文档"，每次发版都要重新跑一遍并更新。
- 报告中的"改进建议"如果已在下一版本完成，需在对应条目后标注 `(已完成于 vX.Y.Z)`。

---

## 六、Git 提交信息规范

### 格式

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type 定义

| Type | 含义 | 使用场景 |
|------|------|----------|
| `feat` | 新功能 | 新增模块、新增输出格式、新增测试套件 |
| `fix` | 修复 | Bug 修复、文案修正、标注错误修正 |
| `docs` | 文档 | README、CHANGELOG、测试报告的更新 |
| `refactor` | 重构 | 不改变行为的代码/结构优化 |
| `test` | 测试 | 新增或修改测试用例 |
| `chore` | 杂项 | 版本号更新、依赖调整、目录结构调整 |

### 示例

```
feat(module-C): add evidence gap list after rhetoric audit

Auto-scan [需顾问补充] and [需顾问判断] tags,
categorize into core vs supporting evidence.

🤖 Generated with [Qoder][https://qoder.com]
```

```
fix(module-D): tone grading default to direct when not specified

Avoid empty output when user omits tone parameter.
```

---

## 七、回归测试套件规范

### 7.1 套件结构

每个技能必须维护一份 `references/regression-test-suite.md`，包含：

1. **单模块功能测试**：每个模块至少 2 个用例（标准模式 + 边界条件）
2. **跨模块工作链测试**：覆盖所有定义的上下文传递路径
3. **通用规则检查**：标注规范、去 AI 味、密度适配、意图路由
4. **测试记录表**：用例 ID、执行日期、结果、阻塞问题、修复版本

### 7.2 用例编号规则

```
TC-{模块}-{序号}    # 单模块用例
TC-X-{序号}         # 跨模块工作链用例
TC-G-{序号}         # 通用规则检查用例
```

### 7.3 执行频率

| 触发条件 | 执行范围 | 执行人 |
|----------|----------|--------|
| 每次 PR | 相关模块的用例 + 通用规则检查 | 开发者自测 |
| 每次发版（Y+1 / X+1） | 全部用例 | 维护者 |
| 每季度 | 全部用例 + 竞品对比抽查 | 维护者 |

---

## 八、技能协同接口规范

当 Sloth 技能需要与其他技能（如 Agent-Reach、Sloth-Sales-Eido、gtm-enterprise-account-planning）协同时，必须在 `references/skill-integration.md` 中定义以下信息：

### 8.1 数据输出格式

```markdown
## PSC-Eido -> gtm-enterprise-account-planning

### 输出数据
- 人物画像：{ name, role, attitude, focus, professional_win, risk, personal_motivation }
- 动作项：{ description, owner, due_date, priority }
- 风险信号：{ description, severity, meddicc_category }

### 注入位置
- 人物画像 -> gtm 组织架构表 + 个人赢点映射
- 动作项 -> MAP 条目
- 风险信号 -> MEDDICC Issues 表

### 触发时机
- 模块 A 完成后，提示用户"是否同步到 gtm？"
- 所有 CRM 写入需用户确认。
```

### 8.2 接口变更管理

- 协同接口的输入/输出格式变更属于**主版本号升级**（X+1）。
- 新增协同技能属于**次版本号升级**（Y+1）。
- 协同文档的文案更新属于**修订号升级**（Z+1）。

---

## 九、已有技能版本状态追踪

| 技能名 | 仓库 | 当前版本 | 最新测试报告 | 状态 |
|--------|------|----------|--------------|------|
| sloth-cpq-eido | Sloth-CPQ-Eido | v1.0.2 | test-report-v1.0.md | 生产可用 |
| sloth-cpq-multi-industry-guide | sloth-cpq-multi-industry-guide | v1.0.0 | test-report-v1.0.md | 生产可用 |
| sloth-psc-eido | Sloth-PSC-Eido | v1.2.0 | test-report-v1.0.md | 生产可用 |
| sloth-sales-eido | Sloth-Sales-Eido | - | - | 待测试 |
| sloth-stratalign-eido | Sloth-StratAlign-Eido | - | - | 待测试 |
| sloth-smm-eido | Sloth-SMM-Eido | - | - | 待测试 |

**维护责任**：
- 每个技能的维护者负责按本规范执行发版检查。
- 跨技能协同接口的变更需通知所有相关技能维护者。

---

## 十、附录：快速参考卡片

### 发版前 5 分钟检查

```
□ version in SKILL.md == latest in CHANGELOG
□ CHANGELOG has Added/Changed/Fixed entries
□ regression tests passed
□ no forbidden AI words in output
□ no orphan files in references/
□ committed and tagged
```

### 版本升级决策树

```
是否移除功能或破坏性变更？
  ├─ 是 → X+1
  └─ 否 → 是否新增功能或模块？
      ├─ 是 → Y+1
      └─ 否 → Z+1
```

---

*慢一点，深一度。*
