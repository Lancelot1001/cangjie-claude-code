# 规则

## 结构

规则按**通用**层和**语言特定**目录组织：

```
rules/
├── README.md              # 本文件
├── cangjie/               # 仓颉语言特定规则
│   ├── README.md          # 仓颉规则说明
│   ├── coding-style.md    # 编码风格
│   ├── testing.md         # 测试要求
│   ├── patterns.md        # 设计模式
│   ├── hooks.md           # Hook 配置
│   ├── security.md        # 安全规范
│   └── agents.md          # Agent 编排
```

## 仓颉语言版本

本规则集针对 **仓颉语言 1.0.5** 版本。

## 安装

### 手动安装

> **重要提示：** 复制整个目录 — 不要使用 `/*` 展开。

```bash
# 创建目标目录
mkdir -p ~/.claude/rules

# 安装仓颉语言规则
cp -r rules/cangjie ~/.claude/rules/cangjie
```

## 规则文件说明

| 文件 | 说明 |
|------|------|
| `coding-style.md` | 仓颉编码风格：let/var 不可变性、函数式编程、模式匹配 |
| `testing.md` | 仓颉测试要求：@test 装饰器、单元测试、覆盖率 |
| `patterns.md` | 仓颉设计模式：Actor 模式、trait、Result/Option |
| `hooks.md` | 仓颉 Hook 配置：格式化、编译检查 |
| `security.md` | 仓颉安全规范：并发安全、类型安全 |
| `agents.md` | 仓颉 Agent 编排：planner/tdd-guide/code-reviewer |

## 规则优先级

当仓颉特定规则与通用规则冲突时，**仓颉特定规则优先**。

详见 [cangjie/README.md](./cangjie/README.md)。