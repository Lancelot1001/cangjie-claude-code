# 仓颉语言规则

> 本文件扩展 common 规则，针对仓颉语言 1.0.5 版本。

## 语言版本

仓颉语言 1.0.5（发布日期：2026/02/10）

## 核心特性

仓颉编程语言是一种面向全场景应用开发的通用编程语言，主要特性：

- **多后端支持**：CJNative（原生二进制）和 CJVM（字节码）
- **语法简明高效**：插值字符串、主构造函数、Flow 表达式、match 和重导出
- **多范式编程**：函数式、命令式、面向对象
- **类型安全**：静态强类型语言
- **内存安全**：自动内存管理，运行时安全检查
- **高效并发**：用户态轻量化线程（原生协程）
- **领域易扩展**：基于词法宏的元编程能力

## 规则文件

| 文件 | 说明 |
|------|------|
| [coding-style.md](./coding-style.md) | 编码风格规范 |
| [testing.md](./testing.md) | 测试要求 |
| [patterns.md](./patterns.md) | 设计模式 |
| [hooks.md](./hooks.md) | Hook 配置 |
| [security.md](./security.md) | 安全规范 |
| [agents.md](./agents.md) | Agent 编排 |

## 开发工具

- **编译器**：`cjc`
- **包管理器**：`cjpm`
- **调试器**：`cjdb`
- **静态检查**：`cjlint`

## 快速开始

```bash
# 初始化项目
cjpm init

# 编译运行
cjpm run
```

## 参考资料

- 官方文档：https://cangjie-lang.cn/docs
- 官方仓库：https://gitcode.com/Cangjie