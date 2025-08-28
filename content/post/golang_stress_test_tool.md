---
title: "Golang 实现的简单性能测试CLI工具源码"
date: 2025-08-27T07:52:03+08:00
draft: false
---

## 项目结构

```
apistress/
├── go.mod
├── main.go                    # 主入口
├── cmd/
│   └── root.go               # CLI 命令定义
└── internal/
    ├── types/
    │   └── config.go         # 数据结构定义
    ├── client/
    │   └── http.go           # HTTP 客户端
    ├── utils/
    │   └── parser.go         # 解析工具函数
    ├── stress/
    │   └── test.go           # 压力测试核心逻辑
    └── output/
        └── printer.go        # 结果输出格式化
```

## 包结构说明

**`main`** - 程序入口点，调用 cmd 包

**`cmd`** - 命令行接口，使用 cobra 处理 CLI 参数

**`internal/types`** - 内部类型定义

- `RequestConfig` - 请求配置
- `TestResult` - 测试结果
- `JSONOutput` - JSON 输出格式

**`internal/client`** - HTTP 客户端封装

- `MakeRequest` - 执行 HTTP 请求

**`internal/utils`** - 工具函数

- `ParseDuration` - 解析时间字符串
- `ParseHeaders` - 解析头部参数

**`internal/stress`** - 压力测试核心

- `RunStressTest` - 主要测试逻辑
- `runWorker` - 工作协程
- `updateResults` - 结果更新

**`internal/output`** - 输出格式化

- `PrintResults` - 结果打印
- `printTextResults` - 文本格式
- `printJSONResults` - JSON 格式

## 使用方法

```bash
# 创建项目目录
mkdir apistress && cd apistress

# 初始化 Go 模块
go mod init apistress

# 创建对应的目录结构
mkdir -p cmd internal/{types,client,utils,stress,output}

# 将对应代码保存到各自文件中
# main.go
# cmd/root.go
# internal/types/config.go
# internal/client/http.go
# internal/utils/parser.go
# internal/stress/test.go
# internal/output/printer.go

# 安装依赖
go mod tidy

# 运行测试
go run . rest --url https://httpbin.org/get --duration 5s

# 编译
go build -o apistress
./apistress rest --url https://httpbin.org/post --method POST --body '{"test":"data"}' --output json
```

## 代码

```golang
// go.mod
module apistress

go 1.21

require (
github.com/spf13/cobra v1.8.0
github.com/schollz/progressbar/v3 v3.14.1
)

require (
github.com/inconshreveable/mousetrap v1.1.0 // indirect
github.com/mitchellh/colorstring v0.0.0-20190213212951-d06e56a500db // indirect
github.com/rivo/uniseg v0.4.4 // indirect
github.com/spf13/pflag v1.0.5 // indirect
golang.org/x/sys v0.14.0 // indirect
golang.org/x/term v0.14.0 // indirect
)

// main.go
package main

import (
"apistress/cmd"
"fmt"
"os"
)

func main() {
if err := cmd.Execute(); err != nil {
fmt.Fprintf(os.Stderr, "Error: %v\n", err)
os.Exit(1)
}
}

// cmd/root.go
package cmd

import (
"apistress/internal/stress"
"apistress/internal/types"
"apistress/internal/utils"
"fmt"
"strings"

    "github.com/spf13/cobra"

)

var rootCmd = &cobra.Command{
Use: "apistress",
Short: "A CLI tool for stress testing REST APIs",
Long: "A powerful CLI tool for stress testing REST APIs with configurable concurrency, rate limiting, and detailed reporting.",
}

var restCmd = &cobra.Command{
Use: "rest",
Short: "Perform REST API stress test",
RunE: runRestStressTest,
}

var (
url string
method string
body string
headers []string
concurrency int
duration string
rate int
output string
)

func init() {
restCmd.Flags().StringVar(&url, "url", "", "Target URL (required)")
restCmd.Flags().StringVar(&method, "method", "GET", "HTTP method")
restCmd.Flags().StringVar(&body, "body", "", "Request body (JSON)")
restCmd.Flags().StringSliceVar(&headers, "header", []string{}, "Headers in format 'Key: Value'")
restCmd.Flags().IntVar(&concurrency, "concurrency", 10, "Number of concurrent requests")
restCmd.Flags().StringVar(&duration, "duration", "10s", "Test duration (e.g., '30s', '5m')")
restCmd.Flags().IntVar(&rate, "rate", 0, "Requests per second limit (0 = unlimited)")
restCmd.Flags().StringVar(&output, "output", "text", "Output format (text|json)")

    restCmd.MarkFlagRequired("url")
    rootCmd.AddCommand(restCmd)

}

func Execute() error {
return rootCmd.Execute()
}

func runRestStressTest(cmd \*cobra.Command, args []string) error {
// Parse duration
testDuration, err := utils.ParseDuration(duration)
if err != nil {
return fmt.Errorf("invalid duration: %w", err)
}
// Parse headers
headerMap, err := utils.ParseHeaders(headers)
if err != nil {
return fmt.Errorf("invalid headers: %w", err)
}
config := &types.RequestConfig{
URL: url,
Method: strings.ToUpper(method),
Body: body,
Headers: headerMap,
}
// Validate output format
if output != "text" && output != "json" {
return fmt.Errorf("invalid output format: %s (must be 'text' or 'json')", output)
}
return stress.RunStressTest(config, concurrency, testDuration, rate, output)
}

// internal/types/config.go
package types

import "time"

type RequestConfig struct {
URL string
Method string
Body string
Headers map[string]string
}

type TestResult struct {
TotalRequests int64 `json:"total_requests"`
SuccessfulReqs int64 `json:"successful_requests"`
FailedReqs int64 `json:"failed_requests"`
MinLatency time.Duration `json:"min_latency_ns"`
MaxLatency time.Duration `json:"max_latency_ns"`
TotalLatency time.Duration `json:"total_latency_ns"`
StatusCodes map[int]int64 `json:"status_codes"`
Errors []string `json:"errors"`
TestDuration time.Duration `json:"test_duration_ns"`
}

type JSONOutput struct {
Summary TestSummary `json:"summary"`
Latency LatencyStats `json:"latency"`
StatusCodes map[int]int64 `json:"status_codes"`
Errors []string `json:"errors"`
}

type TestSummary struct {
TotalRequests int64 `json:"total_requests"`
SuccessfulReqs int64 `json:"successful_requests"`
FailedReqs int64 `json:"failed_requests"`
RequestsPerSecond float64 `json:"requests_per_second"`
TestDurationSec float64 `json:"test_duration_seconds"`
}

type LatencyStats struct {
MinMs float64 `json:"min_ms"`
MaxMs float64 `json:"max_ms"`
AvgMs float64 `json:"avg_ms"`
}

// internal/client/http.go
package client

import (
"apistress/internal/types"
"net/http"
"strings"
"time"
)

func MakeRequest(client *http.Client, config *types.RequestConfig) (time.Duration, int, error) {
start := time.Now()
var reqBody strings.Reader
if config.Body != "" {
reqBody = \*strings.NewReader(config.Body)
}
req, err := http.NewRequest(config.Method, config.URL, &reqBody)
if err != nil {
return time.Since(start), 0, err
}
// Add headers
for key, value := range config.Headers {
req.Header.Set(key, value)
}
// Set default content type for POST/PUT with body
if config.Body != "" && req.Header.Get("Content-Type") == "" {
req.Header.Set("Content-Type", "application/json")
}
resp, err := client.Do(req)
if err != nil {
return time.Since(start), 0, err
}
defer resp.Body.Close()
latency := time.Since(start)
return latency, resp.StatusCode, nil
}

// internal/utils/parser.go
package utils

import (
"fmt"
"strings"
"time"
)

func ParseDuration(durationStr string) (time.Duration, error) {
return time.ParseDuration(durationStr)
}

func ParseHeaders(headerStrs []string) (map[string]string, error) {
headers := make(map[string]string)
for \_, headerStr := range headerStrs {
parts := strings.SplitN(headerStr, ":", 2)
if len(parts) != 2 {
return nil, fmt.Errorf("invalid header format: %s", headerStr)
}
key := strings.TrimSpace(parts[0])
value := strings.TrimSpace(parts[1])
headers[key] = value
}
return headers, nil
}

// internal/stress/test.go
package stress

import (
"apistress/internal/client"
"apistress/internal/output"
"apistress/internal/types"
"context"
"net/http"
"sync"
"sync/atomic"
"time"

    "github.com/schollz/progressbar/v3"

)

func RunStressTest(config _types.RequestConfig, workers int, testDuration time.Duration, rateLimit int, outputFormat string) error {
// Initialize result tracking
result := &types.TestResult{
StatusCodes: make(map[int]int64),
Errors: []string{},
MinLatency: time.Hour, // Initialize to large value
}
var mutex sync.Mutex
var wg sync.WaitGroup
// Create HTTP client
httpClient := &http.Client{
Timeout: 30 _ time.Second,
}
// Setup context for cancellation
ctx, cancel := context.WithTimeout(context.Background(), testDuration)
defer cancel()
// Rate limiter channel
var rateLimiter chan struct{}
if rateLimit > 0 {
rateLimiter = make(chan struct{}, rateLimit)
// Start rate limiter goroutine
go func() {
ticker := time.NewTicker(time.Second / time.Duration(rateLimit))
defer ticker.Stop()
for {
select {
case <-ctx.Done():
return
case <-ticker.C:
select {
case rateLimiter <- struct{}{}:
default:
// Channel full, skip this tick
}
}
}
}()
}
// Progress bar
bar := progressbar.NewOptions(-1,
progressbar.OptionSetDescription("Running stress test..."),
progressbar.OptionSpinnerType(14),
)
// Start workers
for i := 0; i < workers; i++ {
wg.Add(1)
go func() {
defer wg.Done()
runWorker(ctx, httpClient, config, result, &mutex, rateLimiter)
}()
}
// Update progress bar
go func() {
ticker := time.NewTicker(100 \* time.Millisecond)
defer ticker.Stop()
for {
select {
case <-ctx.Done():
return
case <-ticker.C:
bar.Add(1)
}
}
}()
// Wait for all workers to complete
wg.Wait()
bar.Finish()
result.TestDuration = testDuration
return output.PrintResults(result, outputFormat)
}

func runWorker(ctx context.Context, httpClient *http.Client, config *types.RequestConfig, result *types.TestResult, mutex *sync.Mutex, rateLimiter chan struct{}) {
for {
select {
case <-ctx.Done():
return
default:
// Rate limiting
if rateLimiter != nil {
select {
case <-rateLimiter:
// Got permission to make request
case <-ctx.Done():
return
}
}
// Make request
latency, statusCode, err := client.MakeRequest(httpClient, config)
// Update results
mutex.Lock()
updateResults(result, latency, statusCode, err)
mutex.Unlock()
}
}
}

func updateResults(result \*types.TestResult, latency time.Duration, statusCode int, err error) {
atomic.AddInt64(&result.TotalRequests, 1)
if err != nil {
atomic.AddInt64(&result.FailedReqs, 1)
if len(result.Errors) < 10 { // Limit error collection
result.Errors = append(result.Errors, err.Error())
}
} else {
atomic.AddInt64(&result.SuccessfulReqs, 1)
result.StatusCodes[statusCode]++
}
// Update latency stats
result.TotalLatency += latency
if latency < result.MinLatency {
result.MinLatency = latency
}
if latency > result.MaxLatency {
result.MaxLatency = latency
}
}

// calculateAverageLatency calculates the average latency from total latency and request count
func calculateAverageLatency(result \*types.TestResult) time.Duration {
if result.TotalRequests == 0 {
return 0
}
return result.TotalLatency / time.Duration(result.TotalRequests)
}

// internal/output/printer.go
package output

import (
"apistress/internal/types"
"encoding/json"
"fmt"
"time"
)

func PrintResults(result \*types.TestResult, outputFormat string) error {
switch outputFormat {
case "text":
return printTextResults(result)
case "json":
return printJSONResults(result)
default:
return fmt.Errorf("unsupported output format: %s", outputFormat)
}
}

func printTextResults(result \*types.TestResult) error {
fmt.Println("\n=== Stress Test Results ===")
fmt.Printf("Total Requests: %d\n", result.TotalRequests)
fmt.Printf("Successful: %d\n", result.SuccessfulReqs)
fmt.Printf("Failed: %d\n", result.FailedReqs)
fmt.Printf("Test Duration: %.2fs\n", result.TestDuration.Seconds())
if result.TestDuration.Seconds() > 0 {
rps := float64(result.TotalRequests) / result.TestDuration.Seconds()
fmt.Printf("Requests/sec: %.2f\n", rps)
}
if result.TotalRequests > 0 {
avgLatency := calculateAverageLatency(result)
fmt.Println("\nLatency:")
fmt.Printf(" Min: %.2fms\n", float64(result.MinLatency.Nanoseconds())/1e6)
fmt.Printf(" Max: %.2fms\n", float64(result.MaxLatency.Nanoseconds())/1e6)
fmt.Printf(" Avg: %.2fms\n", float64(avgLatency.Nanoseconds())/1e6)
}
if len(result.StatusCodes) > 0 {
fmt.Println("\nStatus Codes:")
for code, count := range result.StatusCodes {
fmt.Printf(" %d: %d\n", code, count)
}
}
if len(result.Errors) > 0 {
fmt.Println("\nSample Errors:")
for i, err := range result.Errors {
if i >= 5 {
break
}
fmt.Printf(" %d: %s\n", i+1, err)
}
}
return nil
}

func printJSONResults(result \*types.TestResult) error {
avgLatency := calculateAverageLatency(result)
avgLatencyMs := float64(avgLatency.Nanoseconds()) / 1e6
var rps float64
if result.TestDuration.Seconds() > 0 {
rps = float64(result.TotalRequests) / result.TestDuration.Seconds()
}
minLatencyMs := float64(result.MinLatency.Nanoseconds()) / 1e6
if result.MinLatency == time.Hour {
minLatencyMs = 0
}
output := types.JSONOutput{
Summary: types.TestSummary{
TotalRequests: result.TotalRequests,
SuccessfulReqs: result.SuccessfulReqs,
FailedReqs: result.FailedReqs,
RequestsPerSecond: rps,
TestDurationSec: result.TestDuration.Seconds(),
},
Latency: types.LatencyStats{
MinMs: minLatencyMs,
MaxMs: float64(result.MaxLatency.Nanoseconds()) / 1e6,
AvgMs: avgLatencyMs,
},
StatusCodes: result.StatusCodes,
Errors: result.Errors,
}
jsonBytes, err := json.MarshalIndent(output, "", " ")
if err != nil {
return fmt.Errorf("failed to marshal JSON: %w", err)
}
fmt.Println(string(jsonBytes))
return nil
}

```
