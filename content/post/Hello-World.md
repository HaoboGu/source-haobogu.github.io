---
title: "Hello World"
date: 2019-08-14T17:48:52+08:00
draft: false
---

# Go

## 变量

### 类型

Go 的基本类型有

```go
bool
string
int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr
byte 
rune
float32 float64
complex64 complex128
```

### 变量声明 & 赋值

`var`用来声明一个变量列表，变量的类型在最后。如果有初始值，可以省略类型

`:=`是声明变量并且赋值，可省略类型名（自动推断），如：

```go
b := false //布尔型
i := 1 //整型
f := 0.618 //浮点型
c := 'a' //字符
s := "hello" //字符串
cp := 3+2i  //复数
i := [3]int{1,2,3} //数组
```

`:=`相当于`var var_name type = val`。一般来说，上面的var语句中，当右边有值的时候，type可以忽略

- `:=`只能用在函数内，`var`可用在函数外

对于Go，可以统一分组声明变量或者常量：

```go
var (
	int_var int
	str_var string
	complex_var complex128
)
const (
	int_const int
  str_const string
)
```

类型转换 - T(v)：

```go
i := float64(int_var)
```



## 逻辑

一般来说，在Go里面，和Java、C/C++相比，都是无需小括号()，大括号{}必须

### 循环

Go里面只有for循环

语法：

```go
// 标准for
for i := 0; i < 10; i++ {
  fmt.Println(i)
}

// 除条件之外另外两个可选
for ; i < 10; {
  i += 1
  fmt.Println(i)
}

// 去掉分号，其实就是while
for i < 1000 {
  i += 1
  fmt.Println(i)
}
```



### 条件

不同的地方在于，可以在条件之前执行一个简单的语句：

```go
// 判断v是否小于lim，之前先赋值v
if v := math.Pow(x, n); v < lim {
  return v
} else {
  return lim
}
```

### switch

Go中只会运行选定的case

```go
switch os := runtime.GOOS; os {
  case "darwin":
  fmt.Println("OS X")
  case "linux":
  fmt.Println("linux")
  default:
  fmt.Printf("%s", os)
}
```

### defer

`defer`会把语句推迟到**外层函数返回之后**执行，每个defer会被压入defer栈，后进先出：

```go
func main() {
	fmt.Println("counting")
	for i := 0; i < 5; i++ {
		defer fmt.Println(i)
	}
	fmt.Println("done")
}

// 结果
counting
done
4
3
2
1
0
```



## 函数

`func`开头，类型放在最后面

```go
// 最后面的float32是返回值类型
func my_func(x int, y float32) float32 {
  return float32(x) + y
}

// 如果多个参数类型相同，只需要在最后写一个
// 可以有多个返回值
func my_func2(x, y, z int) (int, int, int) {
  return x + y, y + z, z + x
}

// 可以在返回值类型处先命名返回值， 然后直接return即可
// !这样子在长函数中可读性不佳
func my_func3(x, y string) (yy, xx string) {
  yy = y
  xx = x
  return
}

```

