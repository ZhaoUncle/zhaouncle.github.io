# 【Golang】for case 循环用户选择


<!--more-->

> 语法点：

1. for 循环 使用 case 函数，使用 break END 可以再输入 exit 的时候退出，也可以用 return。

2. Go语言也支持label(标签)语法：分别是 break label 和 goto label，break label 和 goto label 都能在循环中跳出循环。

   - 2.1 break 的跳转标签 (label) 必须放在循环语句 for 前面，并且在 break label 跳出循环不再执行 for 循环里的代码。如 **案例一** 所示；
     - 不能标签定义在 break 的下面
     - break 标签只能用于for循环；

   - 2.2 goto label 的 label (标签)既可以定义在 for 循环前面（案例三）,也可以定义在 for 循环后面（案例四），当跳转到标签地方时，继续执行标签下面的代码。

     - 但是由于在大项目中不好控制，所以建议能不使用goto label就不要使用；

     - label 在 for 前后位置不一样，得到的退出结果也不一样；

       

### 案例一：break label

```go
package main

import "fmt"

func main() {
END:
	for {
		var text string
		fmt.Print("请输入指令: ")
		fmt.Scan(&text)

		switch text {
		case "add":
			fmt.Println("添加")
		case "modify":
			fmt.Println("修改")
		case "delete":
			fmt.Println("删除")
		case "query":
			fmt.Println("查询")
		case "exit":
			fmt.Println("退出")
			break END
		default:
			fmt.Println("输入指令错误")
		}
	}
}
```



### 案例二：return

```
package main

import "fmt"

func main() {
	for {
		var text string
		fmt.Print("请输入指令: ")
		fmt.Scan(&text)

		switch text {
		case "add":
			fmt.Println("添加")
		case "modify":
			fmt.Println("修改")
		case "delete":
			fmt.Println("删除")
		case "query":
			fmt.Println("查询")
		case "exit":
			fmt.Println("退出")
			return
		default:
			fmt.Println("输入指令错误")
		}
	}
}
```



### 案例一、二 的 Gif 动态图展示：

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/video/QQ20201029-165759-HD.gif" width="800" hegiht="250" align=center/>



### 案例三：goto ，for 循环前面

```go
package main

import "fmt"

func main() {
	a := 1
END:
	for a < 10 {
		if a == 5 {
			a++
			goto END
		}
		fmt.Println(a)
		a++
	}
	fmt.Println(a)
}
```

![image-20201029173405062](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029173405062.png)

### 案例四：goto，for 循环后面

```go
package main

import "fmt"

func main() {
	a := 1
	for a < 5 {
		if a == 3 {
			a++
			goto END
		}
		fmt.Println(a)
		a++
	}
END:
	fmt.Println(a)
}
```

![image-20201029173335334](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201029173335334.png)
