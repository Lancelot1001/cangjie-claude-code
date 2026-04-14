# 安全规范

> 本文件扩展通用安全规范规则，针对仓颉语言 1.0.5 版本。

## 输入验证

### 用户输入验证

```cangjie
// 使用模式匹配验证
func validateEmail(email: String): Bool {
    match (email) {
        s where s.contains("@") && s.contains(".") => true
        _ => false
    }
}

// 使用正则表达式
func validatePhone(phone: String): Bool {
    let pattern = Regex("^1[3-9]\\d{9}$")
    pattern.matches(phone)
}

// 使用 Option 进行安全处理
func parseAge(input: String): Option<Int64> {
    match (input.toInt64()) {
        Some(age) where age > 0 && age < 150 => Some(age)
        _ => None
    }
}
```

### 类型安全

```cangjie
// 避免使用 Any，尽可能使用具体类型
// 错误
let value: Any = "test"
let str = value as String

// 正确
let value: String = "test"

// 必须使用 Any 时，使用类型检查
func process(value: Any): Unit {
    match (value) {
        s: String => println(s)
        n: Int64 => println(n)
        _ => println("Unknown type")
    }
}
```

## 并发安全

### Actor 消息传递

```cangjie
// 使用 Actor 进行并发
actor SafeCounter {
    var count: Int64 = 0

    func increment(): Int64 {
        count += 1
        count
    }

    func decrement(): Int64 {
        count -= 1
        count
    }
}

// 不要在 Actor 外直接修改状态
// 错误
let counter = SafeCounter()
counter.count = 100  // 不安全！

// 正确
counter.increment()  // 通过消息传递
```

### 避免数据竞争

```cangjie
// 使用不可变数据
let data1 = [1, 2, 3]  // 不可变
let data2 = data1.map { x => x * 2 }  // 新数组

// 使用锁（如果需要可变状态）
class ThreadSafeCounter {
    var count: Int64 = 0
    let lock = Mutex()

    func increment(): Int64 {
        lock.lock()
        count += 1
        let result = count
        lock.unlock()
        result
    }
}
```

### 异步安全

```cangjie
// async/await 安全实践
async func fetchData(): Result<Data, String> {
    try {
        let response = await httpGet(url)
        Ok(response)
    } catch (e: Exception) {
        Err(e.message)
    }
}

// 避免在 async 中使用可变全局状态
var globalState = 0  // 不推荐

// 使用 Actor 或局部变量
async func process(): Int64 {
    let localState = 0  // 推荐
    localState + 1
}
```

## 敏感信息处理

### 密钥管理

```cangjie
// 从环境变量读取配置
func getApiKey(): Option<String> {
    env.get("API_KEY")
}

// 不要硬编码密钥
// 错误
const API_KEY = "sk-xxxxx"  // 危险！

// 正确
func loadConfig(): Config {
    let key = match (env.get("API_KEY")) {
        Some(k) => k
        None => throw Exception("API_KEY not set")
    }
    Config(key)
}
```

### 密码处理

```cangjie
// 使用安全的哈希函数
import std.crypto.*

func hashPassword(password: String): String {
    SHA256.hash(password)
}

func verifyPassword(password: String, hash: String): Bool {
    hashPassword(password) == hash
}

// 不要明文存储密码
```

## 错误处理安全

### 避免信息泄露

```cangjie
// 用户友好的错误消息，不暴露内部细节
func handleError(e: Exception): String {
    // 不暴露堆栈跟踪或内部路径
    "操作失败，请稍后重试"
}

// 日志记录详细信息（仅在安全环境中）
func logError(e: Exception): Unit {
    // 服务端日志
    println("Error: ${e.message}, Stack: ${e.stackTrace}")
}
```

### 验证错误处理

```cangjie
// 确保所有错误都被处理
func process(): Result<Data, Error> {
    let result = riskyOperation()?
    // 确保正确处理 Err 情况
    validate(result)?
    Ok(result)
}
```

## 网络安全

### HTTP 安全

```cangjie
// 使用 HTTPS
let url = "https://api.example.com/data"

// 验证证书
let config = HttpConfig(verify: true)

// 不信任用户输入的 URL
func fetchFromUserInput(url: String): Result<Data, String> {
    // 验证 URL 格式
    match (url) {
        u where u.startsWith("http://") => Err("仅支持 HTTPS")
        u where u.startsWith("https://") => httpGet(u)
        _ => Err("无效的 URL")
    }
}
```

### 数据校验

```cangjie
// 验证 API 响应
func parseResponse(json: String): Result<Response, String> {
    match (JSON.parse(json)) {
        Some(data) => {
            // 验证必填字段
            match (data.get("code")) {
                Some(200) => Ok(Response.fromJson(data))
                Some(c) => Err("错误码: ${c}")
                None => Err("缺少 code 字段")
            }
        }
        None => Err("JSON 解析失败")
    }
}
```

## 依赖安全

### 使用 cjpm 管理依赖

```toml
[dependencies]
# 官方包
std = "1.0.5"

# 第三方包 - 检查版本和来源
some-package = "1.0.0"
```

### 定期更新依赖

```bash
# 检查更新
cjpm outdated

# 更新依赖
cjpm update
```

## 代码质量检查清单

在标记工作完成前：

- [ ] 所有用户输入已验证
- [ ] 敏感信息从环境变量读取
- [ ] 使用 Actor 进行并发安全
- [ ] 错误消息不泄露敏感数据
- [ ] API 响应已验证
- [ ] 依赖已更新到安全版本