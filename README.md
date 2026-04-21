# Sloth-Playbook-Eido

Sloth-Eido 系列 Agent Skills 的工程化规范与质量守门助手。

## 用途

当 Sloth-Eido 系列技能需要发版、升级、测试验证或规范化改造时，加载此技能获取统一标准：

- 发版前逐项核对检查清单
- 生成标准格式的测试报告
- 确认版本号升级规则
- 输出符合 Keep a Changelog 的变更日志
- 查询已有技能的版本健康状态

## 核心能力

| 模块 | 触发词 | 说明 |
|------|--------|------|
| 发版检查清单 | 发版 / release / 版本检查 | 四级检查清单（代码/功能/测试/发布） |
| 测试报告模板 | 测试报告 / 报告模板 | 标准六章节测试报告框架 |
| CHANGELOG 规范 | CHANGELOG / 变更日志 | Keep a Changelog 格式 + 示例 |
| 版本号管理 | 版本号 / semver | SemVer 规则 + 升级决策树 |
| 目录结构规范 | 目录结构 / skill 结构 | 标准仓库布局和约束 |
| 回归测试套件 | 回归测试 / 测试套件 | 用例结构和编号规则 |
| Git 提交规范 | Git 提交 / commit 规范 | Conventional Commits 适配版 |
| 技能协同接口 | 协同接口 / integration | 数据交换格式和变更管理 |
| 健康检查 | 健康检查 / 状态追踪 | 已有技能版本状态一览 |
| 快速参考 | 快速参考 / cheatsheet | 5 分钟检查 + 决策树 |

## 仓库结构

```
sloth-playbook-eido/
├── SKILL.md                          # 技能入口
├── README.md                         # 本文件
├── LICENSE                           # MIT
├── references/
│   └── release-playbook.md           # 完整规范文档
└── templates/
    ├── release-checklist.md          # 发版检查清单模板
    ├── test-report-template.md       # 测试报告模板
    └── changelog-template.md         # CHANGELOG 模板
```

## 适用范围

| 技能 | 仓库 | 状态 |
|------|------|------|
| sloth-cpq-eido | [Sloth-CPQ-Eido](https://github.com/wangguobao0215/Sloth-CPQ-Eido) | 已纳入规范 |
| sloth-cpq-multi-industry-guide | [sloth-cpq-multi-industry-guide](https://github.com/wangguobao0215/sloth-cpq-multi-industry-guide) | 已纳入规范 |
| sloth-psc-eido | [Sloth-PSC-Eido](https://github.com/wangguobao0215/Sloth-PSC-Eido) | 已纳入规范 |
| sloth-sales-eido | [Sloth-Sales-Eido](https://github.com/wangguobao0215/Sloth-Sales-Eido) | 待测试 |
| sloth-stratalign-eido | [Sloth-StratAlign-Eido](https://github.com/wangguobao0215/Sloth-StratAlign-Eido) | 待测试 |
| sloth-smm-eido | [Sloth-SMM-Eido](https://github.com/wangguobao0215/Sloth-SMM-Eido) | 待测试 |

## 版本

V1.0.0

## 许可

MIT License.

---

*慢一点，深一度。*
