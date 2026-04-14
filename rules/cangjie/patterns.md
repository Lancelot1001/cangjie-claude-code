# 设计模式

> 本文件扩展通用设计模式规则，针对仓颉语言 1.0.5 版本。

## 常用设计模式

### 单例模式

```cangjie
class Singleton {
    static var instance: Option<Singleton> = None

    private init() {}

    static func getInstance(): Singleton {
        match (instance) {
            Some(s) => s
            None => {
                let s = Singleton()
                instance = Some(s)
                s
            }
        }
    }
}
```

### 工厂模式

```cangjie
interface Shape {
    func draw(): Unit
}

class Circle implements Shape {
    func draw(): Unit {
        println("Draw Circle")
    }
}

class Square implements Shape {
    func draw(): Unit {
        println("Draw Square")
    }
}

enum ShapeType {
    Circle | Square
}

func createShape(type: ShapeType): Shape {
    match (type) {
        Circle => Circle()
        Square => Square()
    }
}
```

### 策略模式

```cangjie
interface SortStrategy<T> {
    func sort(items: Array<T>): Array<T>
}

class QuickSort implements SortStrategy<Int64> {
    func sort(items: Array<Int64>): Array<Int64> {
        // 快速排序实现
        quickSort(items, 0, items.length - 1)
    }
}

class BubbleSort implements SortStrategy<Int64> {
    func sort(items: Array<Int64>): Array<Int64> {
        // 冒泡排序实现
        bubbleSort(items)
    }
}

class Sorter<T> {
    strategy: SortStrategy<T>

    func setStrategy(s: SortStrategy<T>) {
        strategy = s
    }

    func sort(items: Array<T>): Array<T> {
        strategy.sort(items)
    }
}
```

## 仓颉特有模式

### Actor 模式

仓颉语言使用 Actor 模型进行并发编程。

```cangjie
// 定义 Actor
actor BankAccount {
    var balance: Float64 = 0.0

    func deposit(amount: Float64): Float64 {
        balance += amount
        balance
    }

    func withdraw(amount: Float64): Option<Float64> {
        if (balance >= amount) {
            balance -= amount
            Some(balance)
        } else {
            None
        }
    }

    func getBalance(): Float64 {
        balance
    }
}

// 使用 Actor
let account = BankAccount()
account.deposit(1000.0)
let result = account.withdraw(500.0)
match (result) {
    Some(b) => println("余额: ${b}")
    None => println("余额不足")
}
```

### Result/Option 模式

处理可能失败的操作。

```cangjie
// 使用 Result
func divide(a: Float64, b: Float64): Result<Float64, String> {
    if (b == 0.0) {
        Err("除数不能为零")
    } else {
        Ok(a / b)
    }
}

// 使用 ?
func calculate(a: Float64, b: Float64): Result<Float64, String> {
    let result = divide(a, b)?
    Ok(result * 2)
}

// 链式操作
func process(): Result<Int64, String> {
    let a = parseInt("10")?
    let b = parseInt("20")?
    Ok(a + b)
}
```

### trait/接口模式

```cangjie
// 定义 trait
trait Printable {
    func print(): Unit
}

trait Comparable<T> {
    func compareTo(other: T): Int64
}

// 实现 trait
class Person implements Printable, Comparable<Person> {
    name: String
    age: Int64

    init(name: String, age: Int64) {
        this.name = name
        this.age = age
    }

    func print(): Unit {
        println("Name: ${name}, Age: ${age}")
    }

    func compareTo(other: Person): Int64 {
        this.age - other.age
    }
}
```

### 模式匹配最佳实践

```cangjie
// 枚举匹配
enum Message {
    Quit
    Move(x: Int64, y: Int64)
    Write(text: String)
    ChangeColor(r: Int64, g: Int64, b: Int64)
}

func handleMessage(msg: Message): Unit {
    match (msg) {
        Quit => println("Quit")
        Move(x, y) => println("Move to (${x}, ${y})")
        Write(text) => println("Write: ${text}")
        ChangeColor(r, g, b) => println("Color: ${r}, ${g}, ${b}")
    }
}

// 解构匹配
func processPoint(p: (Int64, Int64)): String {
    match (p) {
        (0, 0) => "原点"
        (x, 0) => "X轴上，x=${x}"
        (0, y) => "Y轴上，y=${y}"
        (x, y) => "原点外，(${x}, ${y})"
    }
}
```

### 泛型约束模式

```cangjie
// 使用 where 子句
func printAll<T>(items: Array<T>) where T: Printable {
    for (item in items) {
        item.print()
    }
}

// 泛型函数
func findMax<T>(items: Array<T>): Option<T> where T: Comparable<T> {
    if (items.isEmpty()) {
        None
    } else {
        var max = items[0]
        for (i in 1..<items.length) {
            if (items[i].compareTo(max) > 0) {
                max = items[i]
            }
        }
        Some(max)
    }
}
```

### 函数式编程模式

```cangjie
// 使用 map/filter/reduce
let numbers = [1, 2, 3, 4, 5]

// map: 转换
let doubled = numbers.map { x => x * 2 }

// filter: 过滤
let evens = numbers.filter { x => x % 2 == 0 }

// reduce: 聚合
let sum = numbers.reduce(0) { acc, x => acc + x }

// 链式调用
let result = numbers
    .filter { x => x > 2 }
    .map { x => x * 2 }
    .reduce(0) { acc, x => acc + x }
```

### 尾随 lambda 模式

```cangjie
// 适用场景
list.forEach { item =>
    println(item)
}

list.filter { x => x > 0 }
    .map { x => x.toString() }

// DSL 构建
html {
    head { title("Example") }
    body {
        div { "Hello World" }
    }
}
```

## 代码重构模式

### 提取函数

```cangjie
// 之前
func processOrder(order: Order): Unit {
    // 验证订单
    if (order.items.isEmpty()) {
        throw Exception("订单不能为空")
    }
    // 计算价格
    var total = 0.0
    for (item in order.items) {
        total += item.price * item.quantity
    }
    // 保存订单
    database.save(order)
    // 发送通知
    notify(order.customer)
}

// 之后
func processOrder(order: Order): Unit {
    validateOrder(order)
    let total = calculateTotal(order)
    saveOrder(order)
    notifyCustomer(order)
}
```

### 替换条件表达式

```cangjie
// 之前
func getGrade(score: Int64): String {
    if (score >= 90) {
        "A"
    } else if (score >= 80) {
        "B"
    } else if (score >= 70) {
        "C"
    } else if (score >= 60) {
        "D"
    } else {
        "F"
    }
}

// 之后（使用 match）
func getGrade(score: Int64): String {
    match (score) {
        s where s >= 90 => "A"
        s where s >= 80 => "B"
        s where s >= 70 => "C"
        s where s >= 60 => "D"
        _ => "F"
    }
}
```

## 代码质量检查清单

在标记工作完成前：

- [ ] 使用合适的模式解决常见问题
- [ ] 优先使用不可变数据结构
- [ ] 使用 Result/Option 处理错误
- [ ] 使用 match 替代复杂条件
- [ ] 使用泛型避免重复代码
- [ ] 使用函数式编程风格处理集合