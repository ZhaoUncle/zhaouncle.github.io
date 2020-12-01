# 【Golang】基础-操作 csv 


<!--more-->



# 1. [encoding/csv](https://golang.org/pkg/encoding/csv/)，自带极简

## 1.1 写数据到csv文件

知识点： Write 方法使用`[]string`的切片格式追加方式写入数据

### 1.1.1 追加写入

```go
package main

import (
	"encoding/csv"
	"fmt"
	"log"
	"os"

	"github.com/spf13/cast"
)

type User struct {
	ID   int64
	Name string
	Tel  string
	Addr string
}

func StructToCsv(filename string, UsersDb []User) {
	newFile, err := os.Create(filename)
	if err != nil {
		fmt.Println(err)
	}
	defer func() {
		newFile.Close()
	}()
	// 写入UTF-8
	newFile.WriteString("\xEF\xBB\xBF") // 写入UTF-8 BOM，防止中文乱码
	// 写数据到csv文件
	w := csv.NewWriter(newFile)
	header := []string{"ID", "Name", "Tel", "Addr"} //标题

	w.Write(header)
	for _, v := range UsersDb {
		context := []string{
			cast.ToString(v.ID),
			v.Name,
			v.Tel,
			v.Addr,
		}
		// data = append(data, context)
		w.Write(context)
	}

	if err := w.Error(); err != nil {
		log.Fatalln("error writing csv:", err)
	}
	w.Flush()
}
func main() {
	Users1 := []User{
		{1, "admin", "adminTel", "adminAddr"},
		{2, "test", "testTel", "testAddr"},
	}
	StructToCsv("user.csv", Users1)

}
```



### 1.1.2 一次性写入

知识点：WriteAll 使用[][] `[][]string` 格式一次性写入数据，覆盖 csv 文件

```go
package main

import (
	"encoding/csv"
	"fmt"
	"os"

	"github.com/spf13/cast"
)

type User struct {
	ID   int
	Name string
	Tel  string
	Addr string
}

func StructToCsv(filename string, UsersDb []User) {
	newFile, err := os.Create(filename)
	if err != nil {
		fmt.Println(err)
	}
	defer func() {
		newFile.Close()
	}()
	// 写入UTF-8
	newFile.WriteString("\xEF\xBB\xBF") // 写入UTF-8 BOM，防止中文乱码
	// 写数据到csv文件
	w := csv.NewWriter(newFile)
	header := []string{"ID", "Name", "Tel", "Addr"} //标题
	data := [][]string{
		header,
	}
	for _, v := range UsersDb {
		context := []string{
			cast.ToString(v.ID),
			v.Name,
			v.Tel,
			v.Addr,
		}
		data = append(data, context)
	}
	w.WriteAll(data) // WriteAll方法使用Write方法向w写入多条记录，并在最后调用Flush方法清空缓存。
  
	w.Flush()
}
func main() {
	Users1 := []User{
		{1, "admin", "adminTel", "adminAddr"},
		{2, "test", "testTel", "testAddr"},
	}
	StructToCsv("user.csv", Users1)

}

```

### 1.1.3 执行结果

```go
ID,Name,Tel,Addr
1,admin,adminTel,adminAddr
2,test,testTel,testAddr
```



## 1.2 读取 csv 文件



### 1.2.1 Read，逐条读取为`[]string`格式的数据

```go
package main

import (
	"encoding/csv"
	"fmt"
	"io"
	"log"
	"os"

	"github.com/spf13/cast"
)

type User struct {
	ID   int
	Name string
	Tel  string
	Addr string
}

var Users []*User

func CsvToDb(filename string) {
	f, err := os.Open(filename)
	if err != nil {
		fmt.Println("Error: ", err)
	}

	reader := csv.NewReader(f)
	result1 := make([][]string, 0)
	result2 := make([][]string, 0)
	for {
		record, err := reader.Read()
		if err == io.EOF {
			break
		}
		if err != nil {
			log.Fatal(err)
		}
		result1 = append(result1, record)
	}

	for k, _ := range result1 {
		if result1[k][1] == "Name" && result1[k][2] == "Tel" && result1[k][3] == "Addr" {
			result2 = append(result1[:k], result1[k+1:]...)
		}
	}
	for _, v := range result2 {
		user := &User{
			ID:   cast.ToInt(v[0]),
			Name: v[1],
			Addr: v[2],
			Tel:  v[3],
		}
		Users = append(Users, user)
	}

	for _, v := range Users {
		fmt.Println(v)
	}
}

func main() {
	CsvToDb("user.csv")
}
```



### 1.2.2 ReadAll，一次性读取为`[][]string`格式的数据

```go
package main

import (
	"encoding/csv"
	"fmt"
	"os"

	"github.com/spf13/cast"
)

type User struct {
	ID   int
	Name string
	Tel  string
	Addr string
}

var Users []*User

func CsvToDb(filename string) {
	f, err := os.Open(filename)
	if err != nil {
		fmt.Println("Error: ", err)
	}

	reader := csv.NewReader(f)

	// 可以一次性读完
	result, err := reader.ReadAll()
	if err != nil {
		fmt.Println("Error: ", err)
	}

	result1 := make([][]string, 0)
	for k, _ := range result {
		if result[k][1] == "Name" && result[k][2] == "Tel" && result[k][3] == "Addr" {
			result1 = append(result[:k], result[k+1:]...)
		}
	}
	for _, v := range result1 {
		user := &User{
			ID:   cast.ToInt(v[0]),
			Name: v[1],
			Addr: v[2],
			Tel:  v[3],
		}
		Users = append(Users, user)
	}

	for _, v := range Users {
		fmt.Println(v)
	}
}

func main() {
	CsvToDb("user.csv")
}
```



# 2. [github.com/gocarina/gocsv](https://github.com/gocarina/gocsv)

简单案例：

```go
package main

import (
	"fmt"
	"os"

	"github.com/gocarina/gocsv"
)

type Client struct { // Our example struct, you can use "-" to ignore a field
	ID   string `csv:"ID"`
	Name string `csv:"Name"`
	Tel  string `csv:"Tel"`
	Addr string `csv:"-"`
}

// type Client struct { // Our example struct, you can use "-" to ignore a field
// 	ID   string "ID"
// 	Name string "Name"
// 	Tel  string "Tel"
// 	Addr string "-"
// }

func main() {
	clientsFile, err := os.OpenFile("user.csv", os.O_RDWR|os.O_CREATE, os.ModePerm)
	if err != nil {
		panic(err)
	}
	defer clientsFile.Close()

	clients := []*Client{}

	if err := gocsv.UnmarshalFile(clientsFile, &clients); err != nil { // Load clients from file
		panic(err)
	}
	for _, client := range clients {
		fmt.Println("Hello", client.Name)
	}

	if _, err := clientsFile.Seek(0, 0); err != nil { // Go to the start of the file
		panic(err)
	}

	clients = append(clients, &Client{ID: "12", Name: "John", Tel: "21"}) // Add clients
	clients = append(clients, &Client{ID: "13", Name: "Fred"})
	clients = append(clients, &Client{ID: "14", Name: "James", Tel: "32"})
	clients = append(clients, &Client{ID: "15", Name: "Danny"})
	csvContent, err := gocsv.MarshalString(&clients) // Get all clients as CSV string
	//err = gocsv.MarshalFile(&clients, clientsFile) // Use this to save the CSV back to the file
	if err != nil {
		panic(err)
	}
	fmt.Println(csvContent) // Display all clients as CSV string

}
```

执行结果：

```go
 ~/Documents/projects/scripts/go/eg  cat user.csv
﻿ID,Name,Tel,Addr
1,admin,adminTel,adminAddr
2,test,testTel,testAddr
 ~/Documents/projects/scripts/go/eg  go run 2.go                                      
Hello admin
Hello test
ID,Name,Tel
,admin,adminTel
,test,testTel
12,John,21
13,Fred,
14,James,32
15,Danny,
```




