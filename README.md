# Ketty

[![Go](https://github.com/anqiansong/ketty/actions/workflows/go.yml/badge.svg?branch=main)](https://github.com/anqiansong/ketty/actions/workflows/go.yml)
[![codecov](https://codecov.io/gh/anqiansong/ketty/branch/main/graph/badge.svg?token=KAMZX9OEYF)](https://codecov.io/gh/anqiansong/ketty)
[![Go Report Card](https://goreportcard.com/badge/github.com/anqiansong/ketty)](https://goreportcard.com/report/github.com/anqiansong/ketty)
[![License](https://img.shields.io/github/license/anqiansong/ketty)](https://github.com/anqiansong/ketty/blob/main/LICENSE)

中文|[English](README_EN.md)

ketty 是一个Golang 开发的简单的日志美化输出 Logger。

## 安装

```bash
$ go install github.com/anqiansong/ketty@latest
```

## 快速开始

默认 console 是输出到控制台的，如需要将文件存储到磁盘，请参考下文日志持久化

```go
func main(){
console.Info(`
    {
        "name":"Hello Ketty",
        "description":"a color logger",
        "author":"anqiansong",
        "category":"console",
        "github":"https://github.com/anqiansong/ketty",
        "useage":[
            "info",
            "debug"
        ]
    }`)
console.Debug("Hello Ketty")
console.Warn("Hello Ketty")
console.Error(errors.New("error test"))
}
```

## 终端显示

![terminal](./resource/terminal.png)

## Goland 显示

![idea1](./resource/idea1.png)
![idea1](./resource/idea2.png)

## 用法

### 直接使用

直接使用的 Console 实例支持一些默认配置项：

* 使用 `frame.WithLineStyle` 作为边框
* 默认美化日志
* 不会持久化到文件

```go
func main(){
console.Info("Hello ketty, This is a info log")
console.Debug("Hello ketty, This is a debug log")
console.Warn("Hello ketty, This is a warn log")
console.Error(errors.New("Hello ketty,This is an error"))
}
```

### 初始化

```go
    // 替换默认的边框
plusStyle := text.WithPlusStyle()
c := console.NewConsole(console.WithTextOption(plusStyle))
```

### Console 配置

```go
    c.DisableBorder() // 禁用边框
c.DisableColor() // 禁用颜色美化
```

### 打印 log

```go
    // 输出 info 日志
c.Info("Hello Ketty, It's now %q", time.Now())
```

## 边框样式

### 预设样式

* WithLineStyle 默认样式

```text
[INFO] 2021-11-26 22:29:14.508 ┌────────────────────────────────────────────────────────────────────────────
[INFO] 2021-11-26 22:29:14.508 │  Hello Ketty, It's now "2021-11-26 22:29:14.508085 +0800 CST m=+0.000229190"
[INFO] 2021-11-26 22:29:14.508 └────────────────────────────────────────────────────────────────────────────
```

* WithDotStyle

```text
[INFO] 2021-11-26 22:30:22.913 .............................................................................
[INFO] 2021-11-26 22:30:22.913 .  Hello Ketty, It's now "2021-11-26 22:30:22.913678 +0800 CST m=+0.000199931"
[INFO] 2021-11-26 22:30:22.913 .............................................................................
```

* WithStarStyle

```text
[INFO] 2021-11-26 22:31:00.699 *****************************************************************************
[INFO] 2021-11-26 22:31:00.699 *  Hello Ketty, It's now "2021-11-26 22:31:00.699094 +0800 CST m=+0.000186578"
[INFO] 2021-11-26 22:31:00.699 *****************************************************************************
```

* WithPlusStyle

```text
[INFO] 2021-11-26 22:31:26.952 +----------------------------------------------------------------------------
[INFO] 2021-11-26 22:31:26.952 |  Hello Ketty, It's now "2021-11-26 22:31:26.952376 +0800 CST m=+0.000168647"
[INFO] 2021-11-26 22:31:26.952 +----------------------------------------------------------------------------
```

* WithFivePointedStarStyle

```text
[INFO] 2021-11-26 22:31:58.146 ★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★
[INFO] 2021-11-26 22:31:58.146 ★  Hello Ketty, It's now "2021-11-26 22:31:58.146534 +0800 CST m=+0.000171850"
[INFO] 2021-11-26 22:31:58.146 ★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★
```

* WithDoubleLine

```text
[INFO] 2021-11-26 22:32:21.574 ╔════════════════════════════════════════════════════════════════════════════
[INFO] 2021-11-26 22:32:21.574 ║  Hello Ketty, It's now "2021-11-26 22:32:21.573911 +0800 CST m=+0.000152572"
[INFO] 2021-11-26 22:32:21.574 ╚════════════════════════════════════════════════════════════════════════════
```

* DisableBorder

```text
[INFO] 2021-11-26 22:33:01.695   Hello Ketty, It's now "2021-11-26 22:33:01.695338 +0800 CST m=+0.000156150"
```

### 自定义样式

* WithCommonBorder

```go
// 边框横向、众项、拐角均为一种符号
plusStyle := text.WithCommonBorder("x")
```

```text
[INFO] 2021-11-26 22:34:01.437 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
[INFO] 2021-11-26 22:34:01.437 x  Hello Ketty, It's now "2021-11-26 22:34:01.437286 +0800 CST m=+0.000153825"
[INFO] 2021-11-26 22:34:01.437 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

* WithBorder

```go
    // arg1: 左上角符号
// arg2: 左下角符号
// arg3: 横向边框符号
// arg4: 垂直边框符号
plusStyle := text.WithBorder("=", "=", "-", "|")
c := console.NewConsole(console.WithTextOption(plusStyle))
```

```text
[INFO] 2021-11-26 22:37:38.409 =----------------------------------------------------------------------------
[INFO] 2021-11-26 22:37:38.409 |  Hello Ketty, It's now "2021-11-26 22:37:38.408952 +0800 CST m=+0.000155037"
[INFO] 2021-11-26 22:37:38.409 =----------------------------------------------------------------------------
```

## 日志持久化

你可以通过 WithOutputDir 指定一个日志输出目录，并通过 Flush 进行日志强行写入磁盘，默认情况下， ketty 会每秒钟自动去 Flush 一次。

```go
c := NewConsole(WithOutputDir(dir))
// Don't forget to close it, otherwise the goroutine maybe overflow
defer c.Close()
// 为了防止日志文件急剧增大，可以关闭颜色美化和边框美化，减少不必要的日志输出到文件
c.DisableColor()
c.DisableBorder()
c.Info("It's now: %v", time.Now())

// 手动落盘
c.Flush()
```

## 注意事项

Windows 不支持美化输出。