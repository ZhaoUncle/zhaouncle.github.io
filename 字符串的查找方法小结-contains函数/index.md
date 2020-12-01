# 【Golang】字符串的查找方法小结


<!--more-->



# Contains 函数

1. func Contains(s, substr string) bool

   用途：查找字符串 substr 是否在字符串 s 内，存在就返回 true

   - 精确匹配

   - 区分大小写
   - 字符串必须相连，无法个几个再匹配
   - 空字符串也是匹配项

   ```go
   package main
   
   import (
   	"fmt"
   	"strings"
   )
   
   func main() {
   	fmt.Println(strings.Contains("aomineKing", "ao")) //true
   	fmt.Println(strings.Contains("aomineKing", "ek")) //false
   	fmt.Println(strings.Contains("aomineKing", "aK")) //false
   	fmt.Println(strings.Contains("aomineKing", ""))   //true
   	fmt.Println(strings.Contains("", ""))             //true
   }
   ```

   

2. func ContainsAny(s, chars string) bool

   用途：查找 chars 中任意个字符在 s 中，若存在则返回 true

   - 模糊匹配，只要是一个字符存在，则 true
   - 空字符串不匹配
   - 区分大小写

   ```go
   package main
   
   import (
   	"fmt"
   	"strings"
   )
   
   func main() {
   	fmt.Println(strings.ContainsAny("aomineKing", "k"))    //false
   	fmt.Println(strings.ContainsAny("aomineKing", "ui"))   //true
   	fmt.Println(strings.ContainsAny("aomineKing", "qwe"))  //true
   	fmt.Println(strings.ContainsAny("aomineKing", "uaoK")) //true
   	fmt.Println(strings.ContainsAny("aomineKing", ""))     //false
   	fmt.Println(strings.ContainsAny("", ""))               //false
   }
   ```

   参考：
   
   ```go
   package main
   
   import (
   	"fmt"
   	"strings"
   )
   
   var (
   	coins = 50
   	users = []string{
   		"Matthew", "Sarah", "Augustus", "Heidi", "Emilie", "Peter", "Giana", "Adriano", "Aaron", "Elizabeth",
   	}
   	distribution = make(map[string]int, len(users))
   	rewardRules  = map[string]int{
   		"e": 1,
   		"i": 2,
   		"o": 3,
   		"u": 4,
   	}
   )
   
   func dispatchCoin() int {
   	var leftCoins int
   	for _, name := range users {
   		tempCount := 0
   		for k, v := range rewardRules {
   			tempCount += strings.Count(strings.ToLower(name), k) * v
   		}
   		distribution[name] = tempCount
   	}
   	for k, v := range distribution {
   		fmt.Printf("【%v】分到多少枚【%d】金币\n", k, v)
   		leftCoins += v
   	}
   	return coins - leftCoins
   }
   
   func main() {
   	left := dispatchCoin()
   	fmt.Println("剩下：", left)
   
   }
   ```
   
   结果：
   
   ```go
   【Sarah】分到多少枚【0】金币
   【Heidi】分到多少枚【5】金币
   【Giana】分到多少枚【2】金币
   【Adriano】分到多少枚【5】金币
   【Aaron】分到多少枚【3】金币
   【Elizabeth】分到多少枚【4】金币
   【Matthew】分到多少枚【1】金币
   【Augustus】分到多少枚【12】金币
   【Emilie】分到多少枚【6】金币
   【Peter】分到多少枚【2】金币
   剩下： 10
   ```
   
   

# 参考：

https://www.godoc.org/strings#Contains
