# 变量

## 变量声明

Go 语言是静态类型的，变量声明时必须明确变量的类型。Go语言与其他语言的不同的一个地方在于，Go语言的变量类型在变量后面。

```go
var a int // 如果没有赋值，默认为0
var a int = 1 // 声明时赋值
var a = 1 // 声明时赋值
```

`var a = 1`，因为1是int类型的，所以在赋值时，a自动被确定为int型，所以类型名可以忽略不写，或者可以写为：

```go
a := 1
msg := "Hello"
```

## 简单类型

空值：nil

整形类型： int(取决于操作系统)，int8，int16，int32，int64，uint8，uint16……

浮点数类型：float32，float64

字节类型：byte(等价于uint8)

字符串类型：string

布尔值类型：boolean，(true 或 false)

```go
var a int8 = 10
var c1 byte = 'a'
var b float32 = 12.2
var msg = "Hello Wrold"
ok := false
```

## 字符串

在 Go 语言中，字符串使用UTF8编码，好处在于，如果基本是英文，每个字符占1 byte，和 ASCII 编码是一样的，非常节省空间。如果是中文，一般占3字节。包含中文的字符串的处理方式与纯 ASCII 码构成的字符串有点区别。

``` go
package main

import(
	"fmt"
    "reflect"
)

func main(){
    str1 := "Golang"
    str2 := "Go语言"
    fmt.Println(reflect.TypeOf(str2[2]).Kind()) // uint8
    fmt.Println(str1[2], string(str1[2]))       // 108 l
    fmt.Printf("%d %c\n", str2[2], str2[2])     // 232 è
    fmt.Println("len(str2)：", len(str2))       // len(str2)： 8
}
```

- reflect.TypeOf().Kind() 可以知道某个变量的类型，我们可以看到，字符串是以 byte 数组形式保存的，类型是 uint8，占1个 byte，打印时需要用 string 进行类型转换，否则打印的是编码值。
- 因为字符串是以 byte 数组的形式存储的，所以，`str2[2]` 的值并不等于`语`。str2 的长度 `len(str2)` 也不是 4，而是 8（ Go 占 2 byte，语言占 6 byte）。