# 编码风格

> 本文件扩展通用编码风格规则，针对仓颉语言 1.0.5 版本。

## 不可变性原则（关键）

仓颉语言使用 `let` 和 `var` 区分不可变和可变变量。

### let vs var 使用场景

```cangjie
// 优先使用 let（不可变）
let name = "仓颉"           // 不可变变量，赋值后不可修改
let count = 10              // 不可变变量
let items = [1, 2, 3]       // 不可变变量，数组引用不可变

// 仅在需要修改时使用 var
var total = 0
total = total + 1           // 可变变量可以重新赋值
```

### const 常量

```cangjie
const MAX_SIZE = 100        // 编译时求值的常量
const PI = 3.1415926       // 常量用于命名魔法数字
```

**原则**：
- 优先使用 `let`，仅在确实需要可变时才用 `var`
- 使用 `const` 命名编译时常量
- 不可变数据防止隐藏副作用，使调试更容易

## 命名规范

### 标识符规则

- 普通标识符：由 XID_Start 字符开头，后接 XID_Continue 字符
- 原始标识符：使用反引号 `` `if` ``、`` `while` `` 转义关键字

### 命名风格

```cangjie
// 函数：使用 snake_case 或 camelCase，建议 snake_case
func calculate_total() {}   // 函数名
func getUserName() {}      // 也可以使用驼峰

// 类/结构体/接口：使用 PascalCase
struct UserInfo {}
class DataProcessor {}

// 变量/常量：使用 snake_case
let user_name = "张三"
const max_count = 100

// 枚举成员：使用 PascalCase
enum Status {
    Active | Inactive | Pending
}
```

## 文件组织

### 多文件 > 单文件

```cangjie
// user.cj
package user

public struct User {
    name: String
    age: Int64
}

public func createUser(name: String, age: Int64): User {
    User(name, age)
}
```

```cangjie
// main.cj
package main

import user

main() {
    let user = user.createUser("张三", 25)
    println(user.name)
}
```

### 文件大小

- 典型 200-400 行
- 最多 800 行
- 按功能/领域组织，而非按类型

## 代码格式

### 缩进和空格

```cangjie
// 使用 4 空格缩进
func processData(data: Array<Int64>): Int64 {
    let result = 0
    for (item in data) {
        result += item
    }
    return result
}

// 操作符周围加空格
let sum = a + b           // 不写 let sum=a+b

// 逗号后加空格
let arr = [1, 2, 3]       // 不写 [1,2,3]
```

### 块结构

```cangjie
// if-else 块
if (condition) {
    // 分支 1
} else {
    // 分支 2
}

// match 表达式（推荐使用）
match (value) {
    1 => "one"
    2 => "two"
    _ => "other"
}

// for-in 循环
for (item in collection) {
    println(item)
}
```

## 函数设计

### 函数定义

```cangjie
// 有返回值
func add(a: Int64, b: Int64): Int64 {
    a + b
}

// 无返回值（返回 Unit）
func printMessage(message: String) {
    println(message)
}

// 使用 Flow 表达式简化
func max(a: Int64, b: Int64): Int64 {
    if (a > b) { a } else { b }
}
```

### 参数设计

```cangjie
// 使用命名参数提高可读性
func createUser(name: String, age: Int64, email: String): User {
    User(name, age, email)
}

// 调用时使用命名参数
let user = createUser(name: "张三", age: 25, email: "test@example.com")

// 可选参数使用 Option
func greet(name: Option<String>) {
    match (name) {
        Some(n) => println("Hello, ${n}")
        None => println("Hello, World")
    }
}
```

### 尾随 lambda

```cangjie
// 尾随 lambda 语法
list.filter({ x => x > 0 })
list.map { x => x * 2 }    // 省略括号

// 适用场景：回调、函数式编程
```

## 模式匹配

### match 表达式

```cangjie
// 基本 match
match (status) {
    Active => "活动"
    Inactive => "非活动"
    _ => "未知"
}

// 带条件
match (value) {
    x where x > 10 => "大于10"
    x => "其他"
}

// 解构
match (point) {
    (x: 0, y: 0) => "原点"
    (x, y) => "(${x}, ${y})"
}

// 枚举匹配
enum Result<T, E> {
    Ok(T)
    Err(E)
}

match (result) {
    Ok(value) => println(value)
    Err(e) => println(e)
}
```

## 错误处理

### Result 和 Option

```cangjie
// 使用 Option 处理可能为空的值
func findUser(id: Int64): Option<User> {
    // 返回 Some(user) 或 None
}

// 使用 Result 处理可能失败的操作
func parseNumber(s: String): Result<Int64, String> {
    // 返回 Ok(value) 或 Err(error)
}

// 使用 ? 操作符传播错误
func process(): Result<Int64, String> {
    let num = parseNumber("123")?
    Ok(num * 2)
}
```

### try-catch

```cangjie
try {
    let file = openFile("test.txt")
    // 使用文件
} catch (e: Exception) {
    println("Error: ${e}")
}
```

## 面向对象

### 类定义

```cangjie
class Person {
    name: String
    age: Int64

    // 主构造函数
    init(name: String, age: Int64) {
        this.name = name
        this.age = age
    }

    func greet(): String {
        "Hello, I am ${name}"
    }
}

// 使用
let person = Person("张三", 25)
```

### 接口/trait

```cangjie
interface Drawable {
    func draw(): Unit
}

class Circle implements Drawable {
    radius: Float64

    init(radius: Float64) {
        this.radius = radius
    }

    func draw(): Unit {
        println("Draw circle with radius ${radius}")
    }
}
```

## 并发编程

### Actor 模型

```cangjie
actor Counter {
    var count: Int64 = 0

    func increment(): Int64 {
        count += 1
        count
    }

    func getCount(): Int64 {
        count
    }
}

// 使用
let counter = Counter()
counter.increment()
```

### 异步编程

```cangjie
// async/await
async func fetchData(): Data {
    let response = await httpGet(url)
    parse(response)
}
```

## 代码质量检查清单

在标记工作完成前：

- [ ] 代码可读且命名良好
- [ ] 优先使用 `let` 而非 `var`
- [ ] 函数很小（<50 行）
- [ ] 文件聚焦（<800 行）
- [ ] 没有深层嵌套（使用 match 简化）
- [ ] 正确的错误处理（使用 Result/Option）
- [ ] 没有硬编码值（使用常量或配置）
- [ ] 使用不可变模式