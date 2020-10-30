# 【Golang】字符串与整数的转换然后打印类型


<!--more-->

字符串与 int 的转换然后打印类型，主要使用 **strconv** 函数实现

```go
package main

import (
	"fmt"
	"reflect"
	"strconv"
)

func main() {
	a := 1
	b := "1"
	fmt.Println("a", reflect.TypeOf(a))
	fmt.Println("b", reflect.TypeOf(b))

	a1 := strconv.Itoa(a)
	if b1, err := strconv.Atoi(b); err == nil {
		fmt.Println("a1", reflect.TypeOf(a1))
		fmt.Println("b1", reflect.TypeOf(b1))
	}

}
```



![image-20201029181351355](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029181351355.png)


