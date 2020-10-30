# 【Golang】Map 如何删除元素


<!--more-->

## map 函数如何进行删除内部元素呢？

- 一是使用 append 进行排除不要的元素，重组需要的（案例一）；
- 而是用 index 索引进行排除（案例二），常规我一般用这种方法通用；

### 案例一： append for 循环选中需要的

```go
package main

import (
	"fmt"
)

func main() {
	users := []map[string]string{
		{"id": "1", "name": "test1", "contact": "11234", "address": "Guangdong"},
		{"id": "2", "name": "test2", "contact": "21234", "address": "Shanghai"},
		{"id": "3", "name": "test3", "contact": "31234", "address": "Hangzhou"},
		{"id": "4", "name": "test4", "contact": "41234", "address": "Shenzhen"},
	}
	fmt.Println(users)
	id := "1"
	// 方法1:
	// 把不需要删除 => 定义新的切片 => users，使用 append 进行重新组合
	// tempUsers := []map[string]string{}
	tmpUsers := make([]map[string]string, 0, len(users)-1) // 定义一个临时的 map 函数
	for _, user := range users {
		if user["id"] != id { //这里把不等于的 id 一个一个写入 tmpUsers，
			// fmt.Println(user)
			tmpUsers = append(tmpUsers, user)
		}
		// users = tmpUsers
	}
	fmt.Println(tmpUsers)

}
```

### 

### 案例二：append index 索引排除法

```go
package main

import (
	"fmt"
)

func main() {
	users := []map[string]string{
		{"id": "1", "name": "test1", "contact": "11234", "address": "Guangdong"},
		{"id": "2", "name": "test2", "contact": "21234", "address": "Shanghai"},
		{"id": "3", "name": "test3", "contact": "31234", "address": "Hangzhou"},
		{"id": "4", "name": "test4", "contact": "41234", "address": "Shenzhen"},
	}
	fmt.Println(users)
	id := "1"
	tmpUsers := make([]map[string]string, 0, len(users)-1) // 定义一个临时的 map 函数
	for idx, user := range users {
		if user["id"] == id {
			// fmt.Println(idx)
			tmpUsers = append(users[:idx], users[idx+1:]...)
		}
	}
	fmt.Println(tmpUsers)

}

```



结果示例：

![image-20201030101013875](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201030101013875.png)


