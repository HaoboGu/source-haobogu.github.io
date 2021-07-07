# Rust Basics

---
title: "Rust Basics"
author: "Haobo Gu"
tags: [rust]
date: 2021-07-07T10:28:08+08:00
draft: false
summary: Notes about learning rust(in Chinese)
---

学习[微软Rust教程](https://docs.microsoft.com/zh-cn/learn/paths/rust-first-steps/)的笔记。

## Rust的特点

Rust 是现有系统软件语言（如 C 和 C++）的一种安全替代语言。 与 C 和 C++ 一样，Rust 没有大型运行时或垃圾回收器，这几乎与所有其他现代语言形成了鲜明对比。 但是，与 C 和 C++ 不同的是，Rust 保证了内存安全。 Rust 可以避免很多与在 C 和 C++ 中遇到的内存使用错误相关的 bug。

Rust 有以下优点，非常适合各种应用程序：

- 类型安全：编译器可确保不会将任何操作应用于错误类型的变量。
- 内存安全：Rust 指针（称为“引用”）始终引用有效的内存。
- 无数据争用：Rust 的 borrow 检查器通过确保程序的多个部分不能同时更改同一值来保证线程安全。
- 零成本抽象：Rust 允许使用高级别概念，例如迭代、接口和函数编程，将性能成本控制在最低，甚至不会产生成本。 这些抽象的性能与手工编写的底层代码一样出色。
- 最小运行时：Rust 具有非常小的可选运行时。 为了有效地管理内存，此语言也不具有垃圾回收器。 在这一点上，Rust 非常类似于 C 和 C++ 之类的语言。
- 面向裸机：Rust 可以用于嵌入式和“裸机”编程，因此适合用于编写操作系统内核或设备驱动程序。

## 了解Cargo

Cargo是Rust语言的生成工具和依赖管理器，Cargo为管理Rust程序带来了很多便利。

Cargo 可以为你做许多事情，包括：

- 使用 `cargo new` 命令创建新的项目模板。
- 使用 `cargo build` 编译项目。
- 使用 `cargo run` 命令编译并运行项目。
- 使用 `cargo test` 命令测试项目。
- 使用 `cargo check` 命令检查项目类型。
- 使用 `cargo doc` 命令编译项目的文档。
- 使用 `cargo publish` 命令将库发布到 crates.io。

### 尝试Cargo

首先使用Cargo创建一个新的Rust工程：

```shell
cargo init <project-name>
```

创建完工程之后，使用VSCode打开刚才创建的文件夹，可以看到cargo已经自动为你生成了`cargo.toml`和`src/main.rs`两个文件。

- Cargo.toml 是 Rust 代码库的配置文件，用于管理依赖版本等。如果你有其他语言的经验，可以类比`package.json`或者`go.mod`等
- src 子目录中的 main.rs 文件为当前工程的主入口文件，里面的`fn main()`即为主入口函数

`cargo init`生成的工程是一个可运行的Rust的HelloWorld工程。下面还有一些命令可以尝试：

运行当前工程：

```shell
cargo run
```

编译当前工程：

```shell
cargo build
```

编译当前工程（发布使用）：

```shell
cargo build --release
```

编译完成之后，你可以在`target/debug`和`target/release`目录下看到编译出的可执行文件。

## 语言基础

### 变量

#### 声明变量

使用关键字`let`

```rust
// Declare a variable
let a_number;
    
// Declare a second variable and bind the value
let a_word = "Ten";
    
// Bind a value to the first variable
a_number = 10;

println!("The number is {}.", a_number);
println!("The word is {}.", a_word);
```

#### 变量的可变性

和一般的语言不一样，默认情况下，Rust的变量是**不可变的**：

```rust
let a_number = 10;
a_number = 15; // 此行报错！
```

如果想要变量可变，那么需要使用关键字`mut`:

```rust
let mut a_number = 10;
a_number = 15; // 可以
```

为什么要默认变量是不可变的呢？《The Rust Programming Language》有如下解释：

> 如果一部分代码假设一个值永远也不会改变，而另一部分代码改变了这个值，第一部分代码就有可能以不可预料的方式运行。不得不承认这种 bug 的起因难以跟踪，尤其是第二部分代码只是 **有时** 会改变值。
>
> Rust 编译器保证，如果声明一个值不会变，它就真的不会变。这意味着当阅读和编写代码时，不需要追踪一个值如何和在哪可能会被改变，从而使得代码易于推导。
>
> 不过可变性也是非常有用的。变量只是默认不可变；正如在第二章所做的那样，你可以在变量名之前加 `mut` 来使其可变。除了允许改变值之外，`mut` 向读者表明了其他代码将会改变这个变量值的意图。
>
> -- [变量与可变性 - Rust 程序设计语言](https://kaisery.github.io/trpl-zh-cn/ch03-01-variables-and-mutability.html)

##### 不可变变量和常量的区别：

[变量与可变性 - Rust 程序设计语言](https://kaisery.github.io/trpl-zh-cn/ch03-01-variables-and-mutability.html#变量和常量的区别)

总结一下：

1. 不允许对常量使用 `mut`。常量不光默认不能变，它总是不能变。
2. 声明常量使用 `const` 关键字而不是 `let`，并且 *必须* 注明值的类型
3. 常量只能被设置为常量表达式，而不能是函数调用的结果，或任何其他只能在运行时计算出的值
4. 常量可以在任何作用域中声明，包括全局作用域

#### 变量隐藏

可以重复使用let声明同名的变量，这样的话变量名会被绑定在新的值上面，旧的变量就被“隐藏”了。需要注意的是，**旧变量仍然存在**。

```rust
let a_number = 10;let a_number = 15; // 隐藏上面的变量，但是上面的变量不会被删除，仍存在于内存中println!("{}", a_number); // 15
```

### 数据类型

Rust是静态类型语言。在声明变量时，编译器会自动推断变量类型，但是也可以使用`:`（类似typescript）手动指定变量类型：

```rust
let number1 = 15; // 默认是i32类型let number2: i64 = 15; // 手动声明i64整型
```

#### 数字类型

分为整数、浮点数。具体表见：[数据类型 - Rust 程序设计语言](https://kaisery.github.io/trpl-zh-cn/ch03-02-data-types.html#整型)

默认的整型和浮点数类型为：`i32`和`f64`

#### 布尔类型

`true` or `false`

#### 字符

字符类型为`char`，使用单引号括住：

```rust
let s = 's';let emoji = '😃';
```

#### 字符串

Rust中，有好几种字符串类型：`&str`（字符串引用）, `String`（堆上字符串）等。具体使用还是有区别的。可以参考：[字符串 - Rust 程序设计语言](https://kaisery.github.io/trpl-zh-cn/ch08-02-strings.html)。现在，可以先简单地认为 `String` 是可随程序运行而更改的文本数据。 `&str` 引用是文本数据的不可变视图，不会随着程序运行而改变。

#### 元组tuple

元组是固定长度的分组，使用`(<value1>, <value2>, ...)`表示。每个value的类型可以不一样。获取元组中的元素，可使用`tuple.index`

```rust
// Declare a tuple of three elementslet tuple_e: (char, i32, bool) = ('E', 5i32, true);// Use tuple indexing and show the values of the elements in the tupleprintln!("Is '{}' the {}th letter of the alphabet? {}", tuple_e.0, tuple_e.1, tuple_e.2);
```

### 控制流

#### if else

语法很简单：

```rust
if condition {    } else if another_condition {    } else {    }
```

用的时候，`ifelse`块还可以充当表达式：

```rust
let formal = true;let greeting = if formal { // if used here as an expression    "Good day to you."     // 注意，此处没有分号结尾，即 "Good day to you." 为这个if块的返回值} else {    "Hey!"                 // 返回 “Hey!"};println!("{}", greeting)   // prints "Good day to you."
```

### 复杂数据结构

#### 结构体

使用关键字`struct`定义，结构类型名称采用大写形式。

> Rust 支持三种结构类型：经典结构、元组结构和单元结构。 这些结构类型支持使用各种方式对数据进行分组和处理。
>
> - “经典 [C 结构](https://wikipedia.org/wiki/Struct_(C_programming_language))”最为常用。 结构中的每个字段都具有名称和数据类型。 定义经典结构后，可以使用语法 `<struct>.<field>` 访问结构中的字段。
> - 元组结构类似于经典结构，但字段没有名称。 要访问元组结构中的字段，请使用索引元组时所用的语法：`<tuple>.<index>`。 与元组一样，元组结构中的索引值从 0 开始。
> - “单元结构”最常用作标记。 我们将在了解 Rust 的特征功能时，将深入了解单元结构之所以实用的原因。
>
> 以下代码显示三种结构类型变体的示例定义：
>
> ```rust
> // Classic struct with named fields
> struct Student { name: String, level: u8, pass: bool }
> 
> // Tuple struct with data types only
> struct Grades(char, char, char, char, f32);
> 
> // Unit struct
> struct Unit;
> ```

结构体实例化：

```rust
// Instantiate classic struct, specify fields in random order, or in specified order
let user_1 = Student { name: String::from("Constance Sharma"), remote: true, level: 2 };
let user_2 = Student { name: String::from("Dyson Tan"), level: 5, remote: false };

// Instantiate tuple structs, pass values in same order as types defined
let mark_1 = Grades('A', 'A', 'B', 'A', 3.75);
let mark_2 = Grades('B', 'A', 'A', 'C', 3.25);
```

### 枚举

关键字`enum`。需要注意的是，Rust的枚举类型中，每个值的类型可以不同。这样的话，在使用某个枚举类型时，必须接受其下面所有值的类型：

```rust
enum WebEvent {
    // An enum variant can be like a unit struct without fields or data types
    WELoad,
    // An enum variant can be like a tuple struct with data types but no named fields
    WEKeys(String, char),
    // An enum variant can be like a classic struct with named fields and their data types
    WEClick { x: i64, y: i64 }
}
```



## Rust函数

Rust的函数使用关键字`fn`声明：

```rust
fn main() {
    println!("Hello, world!");
}
```

函数的返回值由`->`确定

在函数体中，**大多数**的语句是分号`;`结尾的。如果不是分号结尾的语句，则有可能是函数的**返回值**！

```rust
fn getFive() -> i32 {
    5
}

```
