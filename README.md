# 仓颉 Claude Code 规则集

[![版本](https://img.shields.io/badge/版本-1.0.5-blue)](https://cangjie-lang.cn)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

仓颉语言（Cangjie）专用的 Claude Code 规则集，为使用仓颉语言开发的开发者提供编码规范、测试指南、设计模式等最佳实践。

## 项目简介

本项目为仓颉语言 1.0.5 版本提供 Claude Code 规则集，参考 [everything-claude-code](https://github.com/Actions-Open-AGI/everything-claude-code) 的规则结构构建。

## 语言版本

- **仓颉语言版本**：1.0.5（发布日期：2026/02/10）
- **文档地址**：https://cangjie-lang.cn/docs

## 规则文件

| 文件 | 说明 |
|------|------|
| [rules/cangjie/coding-style.md](./rules/cangjie/coding-style.md) | 编码风格：let/var 不可变性、函数式编程、模式匹配 |
| [rules/cangjie/testing.md](./rules/cangjie/testing.md) | 测试要求：@test 装饰器、单元测试、覆盖率 |
| [rules/cangjie/patterns.md](./rules/cangjie/patterns.md) | 设计模式：Actor 模式、trait、Result/Option |
| [rules/cangjie/hooks.md](./rules/cangjie/hooks.md) | Hook 配置：格式化、编译检查 |
| [rules/cangjie/security.md](./rules/cangjie/security.md) | 安全规范：并发安全、类型安全 |
| [rules/cangjie/agents.md](./rules/cangjie/agents.md) | Agent 编排：planner/tdd-guide/code-reviewer |

## 快速开始

### 安装

```bash
# 创建目标目录
mkdir -p ~/.claude/rules

# 克隆本仓库（或复制规则目录）
git clone https://github.com/Lancelot1001/cangjie-claude-code.git

# 安装规则
cp -r cangjie-claude-code/rules/cangjie ~/.claude/rules/cangjie
```

### 使用

在项目中创建或更新 `CLAUDE.md` 文件，引用仓颉规则：

```markdown
# Claude Code 项目指南

本项目使用仓颉语言 1.0.5 开发。

## 规则

- 遵循 [~/.claude/rules/cangjie/coding-style.md](./rules/cangjie/coding-style.md) 的编码规范
- 测试要求参考 [~/.claude/rules/cangjie/testing.md](./rules/cangjie/testing.md)
```

## 开发工具

| 工具 | 说明 |
|------|------|
| `cjc` | 编译器 |
| `cjpm` | 包管理器 |
| `cjdb` | 调试器 |
| `cjlint` | 静态分析工具 |

### 常用命令

```bash
# 初始化项目
cjpm init

# 编译运行
cjpm run

# 运行测试
cjpm test

# 静态检查
cjlint .
```

## 示例代码

### Hello World

```cangjie
main() {
    println("你好，仓颉")
}
```

### 使用 let/var

```cangjie
// 优先使用 let（不可变）
let name = "仓颉"
let numbers = [1, 2, 3]

// 仅在需要修改时使用 var
var count = 0
count = count + 1
```

### 模式匹配

```cangjie
match (status) {
    Active => "活动"
    Inactive => "非活动"
    _ => "未知"
}
```

### Actor 并发

```cangjie
actor Counter {
    var count: Int64 = 0

    func increment(): Int64 {
        count += 1
        count
    }
}

let counter = Counter()
counter.increment()
```

## 目录结构

```
cangjie-claude-code/
├── rules/
│   ├── README.md              # 规则目录说明
│   └── cangjie/               # 仓颉语言规则
│       ├── README.md          # 仓颉规则说明
│       ├── coding-style.md    # 编码风格
│       ├── testing.md         # 测试要求
│       ├── patterns.md        # 设计模式
│       ├── hooks.md           # Hook 配置
│       ├── security.md        # 安全规范
│       └── agents.md          # Agent 编排
├── README.md                  # 本文件
└── LICENSE                    # 许可证
```

## 贡献指南

欢迎提交 Issue 和 Pull Request！

1. Fork 本仓库
2. 创建分支 (`git checkout -b feature/xxx`)
3. 提交更改 (`git commit -m 'feat: 添加 xxx'`)
4. 推送分支 (`git push origin feature/xxx`)
5. 创建 Pull Request

## 参考资料

- [仓颉语言官网](https://cangjie-lang.cn)
- [仓颉语言官方文档](https://cangjie-lang.cn/docs)
- [everything-claude-code](https://github.com/Actions-Open-AGI/everything-claude-code)

## 许可证

MIT License - 详见 [LICENSE](./LICENSE) 文件。