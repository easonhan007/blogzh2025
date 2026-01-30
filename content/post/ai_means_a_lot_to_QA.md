---
title: "对于测试同学来说，拿AI做测试小工具简直不要太爽"
date: 2025-08-28T17:33:25+08:00
draft: false
weight: 1
---

## 前因后果

最近有一些做测试使用的小工具的需求，拿 AI 生成了一下，发现确实对测试效率有一定的提升。

前几天需要写一个简单的性能压测工具，自己写的话可能要 1-2 天，估计效果还不太好，于是试着拿 AI 去生成了一下。

为了让工具有足够的并发性能，我选择用 rust 语言来实现。

简单描述了一下需求，claude 就在几分钟之内给出了结果。

## 具体过程

我的提示词非常简单

```
How to use rust to implement this cli feature .

it is a stress test cli

apistress rest --url https://api.example.com/users \
               --method POST \
               --body '{"name":"test"}' \
               --header "Authorization: Bearer TOKEN" \
               --concurrency 50 \
               --duration 30s \
               --rate 1000 \
               --output json
```

不久 claude 给出了第一版的代码。

初版的代码里所有的内容都写在`main.rs`里面，尽管简单，但可维护性还是不强。

于是我再加强了一下。

这是我的提示词。

```
seperate to more files

```

告诉 ai 将代码分成多个文件。

这是后来的文件目录。

```
src/main.rs - Entry point that sets up modules and runs the CLI
src/cli.rs - Command line interface definition using clap
src/config.rs - Request configuration and parsing
src/client.rs - HTTP client functionality
src/results.rs - Test results and reporting
src/rate_limiter.rs - Rate limiting implementation
src/stress_test.rs - Main stress testing orchestration
```

逻辑上清楚多了。

根据代码编译了一下，然后随手写了个 api 试试效果。

api 应用是用 golang+gin 去写的，代码非常简单。

```golang
	r.GET("/ping", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{"message": "pong"})
	})
```

拿 ai 生成的工具去试试效果。

```
stress_test rest --url http://localhost:8080/ping --duration 10s

[00:00:10] Test completed
=== Stress Test Results ===
Total Requests: 273398
Successful: 273398
Failed: 0
Test Duration: 10.00s
Requests/sec: 27339.80

Latency:
  Min: 0.06ms
  Max: 33.02ms
  Avg: 0.31ms

Status Codes:
  200: 273398
```

结果是挺惊艳的，跑出了 27k 的 QPS，性能方面应该是满足我的原始需求的。

## 横向比较

为了横向比较这个工具的性能，我又用 golang 写了个类似功能的 cli 压力测试工具。

用同样的被测应用跑了一下效果。

```
⠏ Running stress test...  [9s]
=== Stress Test Results ===
Total Requests: 29244
Successful: 29244
Failed: 0
Test Duration: 10.00s
Requests/sec: 2924.40

Latency:
  Min: 0.18ms
  Max: 614.15ms
  Avg: 3.41ms

Status Codes:
  200: 29244
```

只跑出了 2.9k 的 QPS，基本上性能差了 10 倍。

## 思考

- AI 的编程能力已经超过了我能力，我绞尽脑汁写的工具在性能上是完全比不上 ai 几分钟生成的结果的
- Golang 的性能比 Rust 要差，但在并发上效果差 10 倍，这是不正常的，所以还是印证了上面的观点，ai 的代码写的更合理，比我要强

对于测试同学来说，我们经常会写一些次抛的工具，这些工具可能在某些测试任务上会给我们带来极大的效率提升，比如造数据工具。

但是测试同学的代码能力有限，时间有限，所以一般情况下，我们是不会去写这些次抛工具的。

这就造成了大部分情况下，我们选择用人工的方式去完成这些重复性的工具。

这就是我们测试经常被诟病测试效率低下的一部分原因了。

现在有了 ai，测试同学哪怕不会写代码，也完全有能力把自己的次抛工具需求给描述清楚，毕竟是一次性或者几次性的工具，功能大都不复杂，对 ai 非常友好，所以几个来回就可以把工具给生成出来。

可以预想到，在不远的将来，测试岗位可能会有减少，但熟练使用 ai 并跟测试结合的新型测试人才，应该是会有一定的市场的。

所以从本质上讲，今后的一段时间，发现问题还是靠人，但是 ai 会加速这个过程。

## 学习时间

是时候看一下这个测试工具中的核心代码了，还是非常有学习的意义的。

Rust 版本的压力测试核心函数：

### 函数签名解析

```rust
pub async fn run_stress_test(
    url: String,                    // 目标URL
    method: String,                 // HTTP方法 (GET, POST, etc.)
    body: Option<String>,           // 可选的请求体
    headers: Vec<String>,           // 请求头列表
    concurrency: usize,             // 并发数
    duration: String,               // 测试持续时间字符串
    rate: Option<u64>,              // 可选的速率限制
    output: OutputFormat,           // 输出格式
) -> Result<()>                     // 返回Result类型
```

### 逐步详细解析

#### 1. 解析和配置阶段

```rust
// 解析持续时间字符串 "30s" -> Duration对象
let test_duration = humantime::parse_duration(&duration)?;

// 创建请求配置对象，包含所有HTTP请求参数
let config = RequestConfig::new(url, method, body, headers)?;

// 创建HTTP客户端（复用连接池）
let client = Client::new();

// 创建共享的测试结果对象
let results = Arc::new(Mutex::new(TestResult::new()));
```

**为什么用 `Arc<Mutex<T>>`？**

- `Arc`：原子引用计数，多个线程可以安全地共享所有权
- `Mutex`：互斥锁，确保同一时间只有一个线程能修改结果

#### 2. 进度条设置

```rust
let pb = create_progress_bar();
pb.set_message("Running stress test...");
```

用户可以看到测试正在进行中的实时反馈。

#### 3. 工作线程创建（关键部分）

```rust
let mut handles = Vec::new();

for _ in 0..concurrency {  // 创建指定数量的并发工作者
    // 为每个工作者克隆必要的资源
    let client = client.clone();        // 克隆HTTP客户端
    let config = config.clone();        // 克隆请求配置
    let results = Arc::clone(&results); // 克隆共享结果的引用

    // 为每个工作者创建独立的速率限制器
    let rate_limiter = rate.map(|rps| RateLimiter::new(rps / concurrency as u64));

    // 生成异步任务
    let handle = tokio::spawn(async move {
        run_worker(client, config, results, rate_limiter).await;
    });

    handles.push(handle);  // 保存任务句柄以便后续控制
}
```

**速率限制分配的重要细节**：

```rust
// 如果总速率是1000 RPS，10个并发工作者
rate.map(|rps| RateLimiter::new(rps / concurrency as u64));
// 每个工作者得到：1000 / 10 = 100 RPS
```

#### 4. 时间控制和清理

```rust
// 让测试运行指定的时间
sleep(test_duration).await;

// 时间到了，强制停止所有工作者
for handle in handles {
    handle.abort();  // 中止异步任务
}
```

**为什么用 `abort()` 而不是优雅关闭？**

- 压力测试需要精确的时间控制
- 优雅关闭可能会延长测试时间
- `abort()` 确保立即停止

#### 5. 结果收集和输出

```rust
pb.finish_with_message("Test completed");

// 获取最终结果并打印
let final_results = results.lock().await;  // 获取互斥锁
final_results.print_results(test_duration, &output)?;
```

### 并发执行流程图

```
主线程                     工作线程1            工作线程2            工作线程N
  |                          |                    |                    |
  ├─ 创建配置和客户端         |                    |                    |
  ├─ 创建共享结果对象         |                    |                    |
  ├─ spawn工作线程1 ────────→ 开始发送请求          |                    |
  ├─ spawn工作线程2 ────────────────────────────→ 开始发送请求          |
  ├─ spawn工作线程N ──────────────────────────────────────────────→ 开始发送请求
  ├─ sleep(duration)         │                    │                    │
  │                         │ 持续发送HTTP请求    │ 持续发送HTTP请求    │ 持续发送HTTP请求
  │                         │ 更新共享结果        │ 更新共享结果        │ 更新共享结果
  ├─ 时间到，abort所有任务 ───→ 被强制停止          │                    │
  ├─ ──────────────────────────────────────────→ 被强制停止          │
  ├─ ────────────────────────────────────────────────────────────→ 被强制停止
  ├─ 收集结果
  └─ 打印统计信息
```

### 内存安全和并发安全

#### Rust 的优势：

```rust
// 编译时保证线程安全
let results = Arc::new(Mutex::new(TestResult::new()));
let results_clone = Arc::clone(&results);  // 安全的共享

// 在工作线程中安全地更新结果
{
    let mut results = results.lock().await;  // 获取锁
    results.add_request(latency, status, error);
}  // 锁自动释放
```

#### 对比其他语言可能的问题：

- **C/C++**：可能的内存泄漏和数据竞争
- **Java**：需要手动管理线程池和同步
- **Go**：需要显式的 channel 通信或锁管理

### 错误处理

```rust
// ? 操作符实现链式错误传播
let config = RequestConfig::new(url, method, body, headers)?;
//                                                        ↑
//                                         如果出错，立即返回错误

final_results.print_results(test_duration, &output)?;
//                                                 ↑
//                                    打印出错也会传播错误
```

这个函数展现了 Rust 异步编程的强大：**零成本抽象**、**内存安全**、**并发安全**，同时保持高性能！
