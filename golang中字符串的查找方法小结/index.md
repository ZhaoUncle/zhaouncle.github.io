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

   

# 参考：

https://www.godoc.org/strings#Contains
