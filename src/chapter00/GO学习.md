# GO学习

go的数据类型

1. 基础类型
   - 数字
   - 字符串
   - 布尔
2. 聚合类型
   - 数组
   - 结构体
3. 引用类型
   - pointer
   - slice
   - map
   - function
   - channel
4. 接口类型

## 基本数据类型

### 整数

有符号整数：int8、int16、int32、int64

无符号整数：uint8、uint16、uint32、uint64

此外还有：int、uint（具体多少位与编译器有关）



### 浮点数

float32、float64



### 复数

complex64、complex128



## 数组、字符串和切片

### 数组

**长度为0的数组在内存中并不占用空间**



### 字符串

Go语言字符串的底层结构在`reflect.StringHeader`中定义：

```go
type StringHeader struct {
    Data uintptr
    Len  int
}
```



### 切片

切片的结构定义，`reflect.SliceHeader`：

```go
type SliceHeader struct {
    Data uintptr
    Len  int
    Cap  int
}
```

切片的一些定义方法：

```go
var (
    a []int               // nil切片, 和 nil 相等, 一般用来表示一个不存在的切片
    b = []int{}           // 空切片, 和 nil 不相等, 一般用来表示一个空的集合
    c = []int{1, 2, 3}    // 有3个元素的切片, len和cap都为3
    d = c[:2]             // 有2个元素的切片, len为2, cap为3
    e = c[0:2:cap(c)]     // 有2个元素的切片, len为2, cap为3
    f = c[:0]             // 有0个元素的切片, len为0, cap为3
    g = make([]int, 3)    // 有3个元素的切片, len和cap都为3
    h = make([]int, 2, 3) // 有2个元素的切片, len为2, cap为3
    i = make([]int, 0, 3) // 有0个元素的切片, len为0, cap为3
)
```

注：**len**表示切片元素的个数，**cap**表示切片对应底层数组的长度，计算方式是从左起到数组的末尾

切片容量大于底层数组容量时，会自动创建一个新的底层数组，取消对原数组的引用





## 函数、方法和接口

### 函数

函数分为具名函数和匿名函数

```go
// 具名函数
func Add(a, b int) int {
    return a+b
}

// 匿名函数
var Add = func(a, b int) int {
    return a+b
}
```

### 方法



### 接口

类型不需要显式声明它实现了某个接口：接口被隐式地实现。多个类型可以实现同一个接口。

实现某个接口的类型（除了实现接口方法外）可以有其他的方法。

一个类型可以实现多个接口。

接口类型可以包含一个实例的引用， 该实例的类型实现了此接口（接口是动态类型）



