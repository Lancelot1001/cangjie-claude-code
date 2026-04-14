# Agent 编排

> 本文件扩展通用 Agent 编排规则，针对仓颉语言 1.0.5 版本。

## 可用 Agent

位于 `~/.claude/agents/`：

| Agent | 用途 | 使用时机 |
|-------|------|---------|
| planner | 实施规划 | 复杂功能、重构 |
| tdd-guide | 测试驱动开发 | 新功能、Bug 修复 |
| code-reviewer | 代码审查 | 写完代码后 |
| security-reviewer | 安全分析 | 提交前 |
| rust-reviewer | 仓颉代码审查 | 仓颉项目 |

## 主动使用 Agent

无需用户提示：
1. 复杂功能请求 — 使用 **planner** agent
2. 代码刚写完/修改 — 使用 **code-reviewer** agent
3. Bug 修复或新功能 — 使用 **tdd-guide** agent

## 仓颉项目特定指南

### 何时使用 planner

- 实现新功能需要多个文件协同
- 需要设计数据结构和算法
- 需要规划模块划分和接口

示例：
```
实现用户认证模块，包括：
- 登录/登出功能
- Token 管理
- 权限验证
```

### 何时使用 tdd-guide

- 添加新功能需要测试
- 修复 bug 需要验证
- 需要确保 80%+ 覆盖率

示例：
```
实现加法函数 add(a: Int64, b: Int64): Int64
使用 TDD 方法
```

### 何时使用 code-reviewer

- 完成功能实现后
- 代码审查前
- 提交前检查

示例：
```
审查刚实现的 User 模块代码
```

## 仓颉特定审查要点

### 类型安全

```cangjie
// 检查是否正确使用类型
// 正确
let value: Int64 = 42

// 避免
let value: Any = 42  // 尽量避免 Any
```

### 不可变性

```cangjie
// 优先使用 let
let name = "仓颉"  // 推荐

// 仅在必要时使用 var
var count = 0
count = count + 1  // 仅在确实需要可变时
```

### 错误处理

```cangjie
// 使用 Result/Option 处理错误
func parse(input: String): Result<Int64, String> {
    match (input.toInt64()) {
        Some(n) => Ok(n)
        None => Err("解析失败")
    }
}

// 避免忽略错误
let result = parse("123")?  // 使用 ? 传播错误
```

### Actor 并发

```cangjie
// 正确使用 Actor
actor Bank {
    var balance: Float64 = 0.0

    func deposit(amount: Float64): Float64 {
        balance += amount
        balance
    }
}

// 避免直接修改 Actor 状态
// 错误
let bank = Bank()
bank.balance = 1000  // 不安全！
```

### 模式匹配

```cangjie
// 使用 match 替代 if-else 链
match (status) {
    Active => "活动"
    Inactive => "非活动"
    Pending => "待处理"
    _ => "未知"
}
```

## 并行任务执行

**始终**使用并行 Task 执行独立操作：

```markdown
# 好：并行执行
并行启动 3 个 agent：
1. Agent 1：认证模块安全分析
2. Agent 2：缓存系统性能审查
3. Agent 3：工具类型检查

# 差：不必要时串行
先 agent 1，然后 agent 2，然后 agent 3
```

## 多视角分析

复杂问题使用分角色 subagent：
- 事实审查员：验证语言特性
- 高级工程师：评估架构设计
- 安全专家：分析潜在风险
- 一致性审查员：检查规范遵循

## 代码质量检查清单

在标记工作完成前：

- [ ] 新功能使用 planner 规划
- [ ] 修复/新功能使用 tdd-guide 驱动
- [ ] 完成后使用 code-reviewer 审查
- [ ] 提交前使用 security-reviewer 检查
- [ ] 遵循仓颉编码规范