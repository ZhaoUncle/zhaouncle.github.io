# 【Golang】类型转换之 cast 包


<!--more-->

Go语言中`cast`包实现了基本数据类型和其字符串表示的相互转换。

# cast包

cast 包实现了基本数据类型与其字符串表示的转换，在Go中轻松安全地从一种类型转换为另一种类型.

更多函数请查看[官方文档](https://github.com/spf13/cast)。

## 用法

Cast提供了一些To_____方法。这些方法将始终返回所需的类型。**如果提供了不会转换为该类型的输入，则将返回该类型的0或nil值**。

Cast还提供了与To_____E相同的方法。这些返回与To_____方法相同的结果，外加一个附加错误，告诉您是否成功转换。使用这些方法，您可以分辨出输入何时与零值匹配或转换失败与返回零值之间的区别。

## string与int类型转换

这一组函数是我们平时编程中用的最多的。

### ToString()

`ToString()`函数用于将**非字符串类型**的整数转换为**字符串**类型，函数签名如下。

示例代码如下：

```go
cast.ToString("mayonegg")         // "mayonegg"
cast.ToString(8)                  // "8"
cast.ToString(8.31)               // "8.31"
cast.ToString([]byte("one time")) // "one time"
cast.ToString(nil)                // ""

var foo interface{} = "one more time"
cast.ToString(foo)                // "one more time"
```

### ToInt()

`ToInt()`函数用于将**非 int类型**数据转换为对应的**int**表示，具体的函数签名如下。

示例代码如下：

```go
cast.ToInt(8)                  // 8
cast.ToInt(8.31)               // 8
cast.ToInt("8")                // 8
cast.ToInt(true)               // 1
cast.ToInt(false)              // 0

var eight interface{} = 8
cast.ToInt(eight)              // 8
cast.ToInt(nil)                // 0
```


