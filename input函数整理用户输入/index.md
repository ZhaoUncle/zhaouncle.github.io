# 【Golang】自定义 input 函数整理用户输入


<!--more-->

```go
package main

import (
	"fmt"
)

func input(prompt string) string {
	var text string
	fmt.Print(prompt)
	fmt.Scan(&text)
	return text
}

func main() {
	a := input("你输入的是啥：")
	b := input("还是 python 简单很多：")
	c := input("学会 go 好处多多多哦：")
	fmt.Printf("%v, %v, %v", a, b, c)
}
```



![image-20201030182110173](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201030182110173.png)
