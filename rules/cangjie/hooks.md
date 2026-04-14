# Hook 配置

> 本文件扩展通用 Hook 配置规则，针对仓颉语言 1.0.5 版本。

## 仓颉工具链

仓颉语言提供以下开发工具：

- **cjc**：编译器
- **cjpm**：包管理器
- **cjdb**：调试器
- **cjlint**：静态分析工具

## PreToolUse Hooks

### 编译前检查

```json
{
  "matcher": {
    "tool_name": "Write"
  },
  "hooks": [
    {
      "type": "command",
      "command": ["cjc", "--check", "${file_path}"],
      "working_directory": "${workspace_dir}",
      "execution": "immediate",
      "on_failure": "continue"
    }
  ]
}
```

### 保存时格式化

```json
{
  "matcher": {
    "tool_name": "Write",
    "file_pattern": "**/*.cj"
  },
  "hooks": [
    {
      "type": "command",
      "command": ["cjformat", "-w", "${file_path}"],
      "working_directory": "${workspace_dir}",
      "execution": "immediate"
    }
  ]
}
```

## PostToolUse Hooks

### 编译后运行测试

```json
{
  "matcher": {
    "tool_name": "Bash",
    "command_pattern": "cjpm test"
  },
  "hooks": [
    {
      "type": "command",
      "command": ["cjlint", "."],
      "working_directory": "${workspace_dir}",
      "execution": "deferred"
    }
  ]
}
```

### 构建后检查

```json
{
  "matcher": {
    "tool_name": "Bash",
    "command_pattern": "cjc.*\\.cj"
  },
  "hooks": [
    {
      "type": "command",
      "command": ["cjlint", "${file_path}"],
      "working_directory": "${workspace_dir}",
      "execution": "deferred"
    }
  ]
}
```

## 常用工具配置

### cjlint 配置

创建 `.cjlint.json` 配置文件：

```json
{
  "rules": {
    "enabled": true,
    "categories": [
      "style",
      "correctness",
      "performance"
    ]
  },
  "excludes": [
    "**/test/**",
    "**/examples/**"
  ]
}
```

### cjpm 配置

创建 `cjpm.toml` 配置文件：

```toml
[package]
name = "my-project"
version = "1.0.0"
edition = "1.0.5"

[dependencies]
std = "1.0.5"

[dev-dependencies]
```

## IDE 集成

### VS Code 配置

```json
{
  "cangjie.languageServer.enabled": true,
  "cangjie.format.onSave": true,
  "cangjie.lint.run": "onSave"
}
```

### 常用快捷键

| 操作 | 快捷键 |
|------|--------|
| 格式化文档 | Shift+Alt+F |
| 运行测试 | Ctrl+Shift+T |
| 编译项目 | Ctrl+Shift+B |

## 质量检查工作流

### 开发时

1. 编写代码时使用 `cjlint` 检查
2. 保存时自动格式化（如果配置了）
3. 运行 `cjpm test` 验证功能

### 提交前

```bash
# 运行完整检查
cjpm check
cjpm test
cjlint .
```

### CI/CD 集成

```yaml
# .github/workflows/test.yml
name: Test
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install Cangjie
        run: |
          tar xvf cangjie-sdk-linux-x64-1.0.5.tar.gz
          source cangjie/envsetup.sh
      - name: Run tests
        run: cjpm test
      - name: Lint
        run: cjlint src/
```

## 代码质量检查清单

在标记工作完成前：

- [ ] 配置了 cjlint 静态检查
- [ ] 配置了格式化工具
- [ ] 设置了测试运行命令
- [ ] 配置了 CI/CD 工作流