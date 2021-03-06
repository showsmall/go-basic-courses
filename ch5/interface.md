### 接口

接口类型是一种抽象类型，是方法的集合，其他类型实现了这些方法就是实现了这个接口。

``` go
/* 定义接口 */
type interface_name interface {
   method_name1 [return_type]
   method_name2 [return_type]
   method_name3 [return_type]
   ...
   method_namen [return_type]
}
```

#### 简单示例：打印不同几何图形的面积和周长
``` go
package main

import (
	"fmt"
	"math"
)

type geometry interface {
	area() float32
	perim() float32
}

type rect struct {
	len, wid float32
}

func (r rect) area() float32 {
	return r.len * r.wid
}

func (r rect) perim() float32 {
	return 2 * (r.len + r.wid)
}

type circle struct {
	radius float32
}

func (c circle) area() float32 {
	return math.Pi * c.radius * c.radius
}

func (c circle) perim() float32 {
	return 2 * math.Pi * c.radius
}

func show(name string, param interface{}) {
	switch param.(type) {
	case geometry:
		// 类型断言
		fmt.Printf("area of %v is %v \n", name, param.(geometry).area())
		fmt.Printf("perim of %v is %v \n", name, param.(geometry).perim())
	default:
		fmt.Println("wrong type!")
	}
}

func main() {
	rec := rect{
		len: 1,
		wid: 2,
	}
	show("rect", rec)

	cir := circle{
		radius: 1,
	}
	show("circle", cir)

	show("test", "test param")
}
```

### 接口中可以内嵌接口
对上述例子做以下修改：
- 首先添加 `tmp` 接口，该接口定义了 `area()` 方法
- 将 `tmp` 作为 `geometry` 接口中的匿名成员，并且将 `geometry` 接口中原本定义的 `area()` 方法删除

> 完成以上两步后，`geometry` 接口将会拥有 `tmp` 接口所定义的所有方法。运行结果和上述例子相同。

``` go
type tmp interface{
	area() float32
}

type geometry interface {
	// area() float32
	tmp
	perim() float32
}
```