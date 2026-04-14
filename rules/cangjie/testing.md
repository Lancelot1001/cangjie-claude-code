# 测试要求

> 本文件扩展通用测试要求规则，针对仓颉语言 1.0.5 版本。

## 测试框架

仓颉语言使用 `@test` 装饰器进行单元测试。

### 基本测试结构

```cangjie
package test_example

import stdtest.*

@test
func testBasicAddition() {
    let result = 2 + 2
    @assert(result == 4)
}

@test
func testStringOperations() {
    let str = "Hello"
    @assert(str.length == 5)
    @assert(str.toUppercase() == "HELLO")
}
```

### 运行测试

```bash
# 使用 cjpm 运行测试
cjpm test

# 或使用 cjc 编译并运行
cjc --test your_file.cj
```

## 测试类型

### 单元测试

```cangjie
@test
func testMathFunctions() {
    // 测试绝对值
    @assert(abs(-5) == 5)

    // 测试最大值
    @assert(max(3, 7) == 7)

    // 测试最小值
    @assert(min(3, 7) == 3)
}
```

### 集成测试

```cangjie
@test
func testFileOperations() {
    // 测试文件读写
    let path = "test.txt"
    writeFile(path, "test content")
    let content = readFile(path)
    @assert(content == "test content")
    deleteFile(path)
}
```

### 异步测试

```cangjie
@test
async func testAsyncOperation() {
    let result = await fetchData()
    @assert(result.isNotEmpty())
}
```

## 测试组织

### 文件结构

```
src/
├── math/
│   ├── math.cj          # 实现
│   └── math_test.cj     # 单元测试
├── data/
│   ├── data.cj
│   └── data_test.cj
└── main.cj
```

### 测试命名

```cangjie
// 测试文件：xxx_test.cj
// 测试函数：testXXX

@test
func testArrayFilter() {
    let arr = [1, 2, 3, 4, 5]
    let result = arr.filter { x => x > 2 }
    @assert(result == [3, 4, 5])
}

@test
func testMapOperations() {
    let map = HashMap<String, Int64>()
    map.put("a", 1)
    @assert(map.get("a") == 1)
}
```

## 断言

### 基本断言

```cangjie
// 相等断言
@assert(actual == expected)

// 不等断言
@assert(result != expected)

// 布尔断言
@assert(items.isEmpty())

// 空值断言
@assert(value.isNotNone())
```

### 复杂断言

```cangjie
// 检查异常
@test
func testExceptionHandling() {
    let caught = false
    try {
        throw Exception("error")
    } catch (e: Exception) {
        caught = true
    }
    @assert(caught)
}

// 检查类型
@test
func testTypeChecking() {
    let value: Any = "test"
    @assert(value is String)
}
```

## Mock 和 Stub

### 使用接口进行 Mock

```cangjie
// 定义接口
interface Database {
    func query(sql: String): Array<Row>
    func execute(sql: String): Int64
}

// 实现 Mock
class MockDatabase implements Database {
    var mockData: Array<Row> = []

    func query(sql: String): Array<Row> {
        mockData
    }

    func execute(sql: String): Int64 {
        0
    }

    public func setMockData(data: Array<Row>) {
        mockData = data
    }
}

// 在测试中使用
@test
func testDatabaseQuery() {
    let db = MockDatabase()
    db.setMockData([Row("id": 1), Row("id": 2)])

    let results = db.query("SELECT * FROM users")
    @assert(results.length == 2)
}
```

## 测试覆盖率

### 覆盖率要求

- **最低覆盖率**：80%
- 单元测试覆盖核心逻辑
- 集成测试覆盖关键路径

### 查看覆盖率

```bash
cjpm test --coverage
```

## 测试最佳实践

### AAA 模式

```cangjie
// Arrange - 准备
let calculator = Calculator()
let input = 10

// Act - 执行
let result = calculator.double(input)

// Assert - 断言
@assert(result == 20)
```

### 测试独立性

```cangjie
// 每个测试独立，不依赖其他测试
@test
func testIndependent1() {
    let list = ArrayList<Int64>()
    list.add(1)
    @assert(list.length == 1)
}

@test
func testIndependent2() {
    let list = ArrayList<Int64>()
    @assert(list.isEmpty())
}
```

### 描述性测试名

```cangjie
// 清晰描述测试内容
@test
func testFilterRemovesNegativeNumbers() {
    let arr = [-1, 0, 1, 2]
    let result = arr.filter { x => x >= 0 }
    @assert(result == [0, 1, 2])
}

@test
func testMapTransformsAllElements() {
    let arr = [1, 2, 3]
    let result = arr.map { x => x * 2 }
    @assert(result == [2, 4, 6])
}
```

## 测试驱动开发（TDD）

### 工作流程

1. **写测试**（RED）- 测试应该失败
2. **实现**（GREEN）- 测试应该通过
3. **重构**（IMPROVE）- 保持测试绿色时改进

```cangjie
// Step 1: 先写测试（会失败）
@test
func testFactorial() {
    @assert(factorial(5) == 120)
}

// Step 2: 实现通过测试
func factorial(n: Int64): Int64 {
    if (n <= 1) { 1 }
    else { n * factorial(n - 1) }
}

// Step 3: 重构优化（可选）
```

## 代码质量检查清单

在标记工作完成前：

- [ ] 核心功能有单元测试
- [ ] 关键路径有集成测试
- [ ] 测试覆盖率达到 80%+
- [ ] 测试名称描述清晰
- [ ] 测试独立且可重复
- [ ] 使用 AAA 模式组织测试
- [ ] Mock 依赖进行隔离测试