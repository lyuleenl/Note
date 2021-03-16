# Go语言结构

```GO
package main//声明了 main.go 所在的包，Go 语言中使用包来组织代码。一般一个文件夹即一个包，包内可以暴露类型或方法供其他包使用。

import "fmt"//fmt 是 Go 语言的一个标准库/包，用来处理标准输入输出。

func main() {//main 函数是整个程序的入口，main 函数所在的包名也必须为 main。

  fmt.Println("Hello go")

}
```

## 变量声明

Go 语言是静态类型的,变量声明时必须明确变量的类型

Go 语言与其他语言显著不同的一个地方在于，Go 语言的类型在变量后面

```Go
var a int
var a int =10
var b byte ='g'
var c int8 = 2
var d float32 =12.1
var e bool = true
```

## 变量类型

变量赋值有多种方法

```Go
var b=1//自动确定类型
c:=3
d:="this is go"//简写方式
```

空值：nil

整型类型： int(取决于操作系统), int8, int16, int32, int64, uint8, uint16, …

浮点数类型：float32, float64

字节类型：byte (等价于uint8)

字符串类型：string

布尔值类型：boolean，(true 或 false)

### 字符串

```go
str1 := "Golang"
str2 := "Go语言"
fmt.Println(reflect.TypeOf(str2[2]).Kind()) // uint8   reflect.TypeOf().Kind() 可以知道某个变量的类型，我们可以看到，字符串是以 byte 数组形                                                        式保存的，类型是 uint8，占1个 byte，打印时需要用 string 进行类型转换，否则打印的是编码                                                        值。
fmt.Println(str1[2], string(str1[2]))       // 108 l
fmt.Printf("%d %c\n", str2[2], str2[2])     // 232 è   因为字符串是以 byte 数组的形式存储的，所以，str2[2] 的值并不等于语。str2 的长度                                                              len(str2) 也不是 4，而是 8（ Go 占 2 byte，语言占 6 byte）。
fmt.Println("len(str2)：", len(str2))       // len(str2)： 8
```

正确的处理方法

```go
str2 := "Go语言"

runeArry:=[]rune(str2)
fmt.Println(reflect.TypeOf(runeArry[2]).Kind())//int32
fmt.Println(runeArry[2],string(runeArry[2]))//35821 语
fmt.Println("len(runeArry):",len(runeArry))//len(runeArry): 4
//转换成 []rune 类型后，字符串中的每个字符，无论占多少个字节都用 int32 来表示，因而可以正确处理中文。
```

### 数组

#### 声明

```GO
var arr1 [5]int
var arr2 [2][2]int
arr3:=[3]string{"this","is","go"}
//var arr4 []int
for i := 0; i < 2; i++ {
	arr1[i]+=10
	arr2[i][0]+=10
	//arr4[i]+=10
}
fmt.Println(arr1,arr2,arr3)//[10 10 0 0 0] [[10 0] [10 0]] [this is go]
```

go 的数组可直接输出不需遍历

#### 切片

数组的长度不能改变，如果想拼接2个数组，或是获取子数组，需要使用切片。切片是数组的抽象。 切片使用数组作为底层结构。**切片包含三个组件：容量，长度和指向底层数组的指针,切片可以随时进行扩展**

```go
slice1:=make([]float32,0)//长度为0的切片
slice2:=make([]float32,3,5)//长度为3容量为5的切片
fmt.Println(len(slice1),cap(slice2))//0 5
```

添加元素，切片容量可以根据需要自动扩展

```go
sub1:=slice1[3:]//[1 2 3 4]
sub2:=slice1[:3]//[0 0 0]
sub3:=slice1[1:4]//[0 0 1]
combined:=append(sub1,sub2...)//[1 2 3 4 0 0 0]
fmt.Println(sub1,sub2,sub3,combined)
```

- 声明切片时可以为切片设置容量大小，为切片预分配空间。在实际使用的过程中，如果容量不够，切片容量会自动扩展。
- `sub2...` 是切片解构的写法，将切片解构为 N 个独立的元素。*???????????*

## 循环

go 没有while do while循环

使用for来代替

```GO
//输出10次hello,world(使用类似while循环形式，先判断后做)
func jobWhileMoni() {
    var count = 0
    for {
        if count >= 10 {
            break //如果count>=10则退出
        }
        fmt.Println("hello,world", count)
        count++
    }
}
//模拟do……while实现输出10次hello,world（先做后判断）
func jobDowhileMoni(){
    var i = 0
    for{
        fmt.Println("hello,world",i)
        i++
        if(i>=10){
            break
        }
    }
}
```



# 坑

fmt.println

l是L的小写，不是大写的i!!! 这个坑差点劝退我