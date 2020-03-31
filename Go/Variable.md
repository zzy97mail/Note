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

正确处理方式是将string转为rune数组

```go
str2 := "Go语言"
runeArr := []rune(str2)
fmt.Println(reflect.TypeOf(runeArr[2]).Kind()) // int32
fmt.Println(runeArr[2],string(runeArr[2])) // 35821 语
fmt.Println("len(runeArr)：",len(runeArr)) // len(runeArr)：4
```

转换成`[]rune`类型后，字符串中的每个字符，无论占多少个字节都用int32来表示，因而可以正确处理中文。

## 数组与切片

声明数组

```go
var arr [5]int // 一维数组
var arr2 [5][5]int //二维数组
```

声明时的初始化

```go
var arr = [5]int{1, 2, 3, 4, 5}
arr2 := [5]int{1, 2, 3, 4, 5}
```

使用`[]`索引/修改数组

```go
arr := [5]int{1, 2, 3, 4, 5}
for i := 0; i < len(arr); i++{
    arr[i] += 100
}
fmt.Println(arr) // [101, 102, 103, 104, 105]
```

数组的长度不能改变，如果想拼接两个数组，或者获取子数组，需要使用切片。切片是数组的抽象。切片使用数组作为底层结构。切片包含三个组件：容量，长度和指向底层数组的指针，切片可以随时进行扩展

声明切片：

```go
slice1 := make([]float32, 0) // 长度为0的切片
slice2 := make([]float32, 3, 5) // [0 0 0]长度为3容量为5的切片
fmt.Println(len(slice2), cap(slice2)) // 3 5
```

使用切片：

```go
slice2 = append(slice2, 1, 2, 3, 4) // [0,0,0,1,2,3,4]
fmt.Println(len(slice2),cap(slice2)) // 7 12
// 子切片 [start, end]
sub1 := slice2[3:] // [1,2,3,4]
sub2 := slice2[:3] // [0,0,0]
sub3 := slice2[1:4] // [0,0,1]
// 合并切片
combined := append(sub1, sub2...) // [1,2,3,4,0,0,0]
```

- 生命切片时可以为切片设置容量大小，为切片预分配空间。在实际使用过程中，如果容量不够，切片容量会自动扩展。
- `sub2...`是切片解构的写法，将切片解构为N个独立的元素。

## 字典

map 类似于java的HashMap，Python的字典(dict)，是一种储存键值对的数据结构。使用方式与其他语言几乎没有区别。

```go
// 声明
m1 := make(map[string]int)
// 声明并初始化
m2 := map[string]string{
    "name":"zzy",
    "age":"23",
}
// 赋值/修改
m1["Demo"] = 2
```

## 指针

指针即某个值的地址，类型定义时使用符号`*`，对一个已经存在的变量，使用`&`获取该变量的地址。

```go
str := "Golang"
var p *string = &str // p 是指向 str 的指针
*p = "Hello"
fmt.Println(str) // Hello
```

一般来说，指针通常在函数传递参数，或者给某个类型定义新的方法时使用。Go语言中，参数是按值传递的。如果不使用指针，函数内部将会拷贝一份参数的副本，对参数的修改并不会影响到外部变量的值。如果参数使用指针，对参数的传递将会影响外部变量。

```go
func add(num int){
    num += 1
}
func realAdd(num *int){
    *num += 1
}
func main(){
    num := 100
    add(num)
    fmt.Println(num) // 100,没有变化
    
    realAdd(&num)
    fmt.Println(num) // 101,发生变化
}
```

