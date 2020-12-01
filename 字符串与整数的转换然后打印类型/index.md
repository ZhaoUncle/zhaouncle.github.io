# 【Golang】字符串与整数的转换并打印类型


<!--more-->

## 1. strconv

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
	fmt.Printf("a type of %T\n", a)
	fmt.Printf("b type of %T\n", b)

	a1 := strconv.Itoa(a)
	if b1, err := strconv.Atoi(b); err == nil {
		fmt.Println("a1", reflect.TypeOf(a1))
		fmt.Println("b1", reflect.TypeOf(b1))
	}

}
```

执行结果：

```
a int
b string
a type of int
b type of string
a1 string
b1 int
```



## 2. cast

用 strconv.Atoi 的方法，即将字符串转换为整数是要设置 err 错误信息的，那怎么可以省略呢？

那就是使用 cast 函数

```go
package main

import (
	"fmt"
	"reflect"
	"strconv"

	"github.com/spf13/cast"
)

func main() {
	a := 1
	b := "1"
	
	a2 := cast.ToString(a)
	fmt.Println("a2", reflect.TypeOf(a2))
	b2 := cast.ToInt(b)
	fmt.Println("b2", reflect.TypeOf(b2))

}

```


