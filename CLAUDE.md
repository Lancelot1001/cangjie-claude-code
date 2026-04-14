# Claude Code 项目指南

本项目使用仓颉语言 1.0.5 开发。

## 规则

- 遵循 [rules/cangjie/coding-style.md](rules/cangjie/coding-style.md) 的编码规范
- 测试要求参考 [rules/cangjie/testing.md](rules/cangjie/testing.md)
- 设计模式参考 [rules/cangjie/patterns.md](rules/cangjie/patterns.md)

## 开发工具

- cjc：编译器
- cjpm：包管理器
- cjdb：调试器
- cjlint：静态分析工具

## 常用命令

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