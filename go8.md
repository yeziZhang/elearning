查看程序依赖库  ldd server

go run main.go  合并操作 go build main.go && ./main
# 2 golang的开服环境
GOPATH路径
bin 编译后端程序
pkg 存储一些依赖文件
src 全部的源码 github.com开源库 golang.org
# 3 常见的四种变量声明
:= 只能用函数体内, 全局变量不能用，必须用 var a = 10;

# 7 函数多返回值
函数返回值 可以有多个
``
//第一种匿名返回
func(a int, b int)(int,int){
	fmt.Println("a = ", a)
	fmt.Println("b = ", b)
	return 100,200;
}
//第二种 有名的返回
func(a int, b int)(r1 int, r2 int){
	r1 = 100
	r2 = 200
	return
}
``

# 8 import 导包路径问题 与 init 方法调用流程
main 函数不是第一个执行的函数，都要先去执行 导入包里的init的函数
GOPATH 下 一个目录就是一个工程，工程内的文件夹就是一个包路径，默认main 函数在 package main 包里，GOPATH模式下，导包必须从项目目录开始写知道包的

# 9 import 匿名 和 别名 导包方式
``
package main
import (
	_ "GolangStudy/5-init/lib1"			//匿名导包，无法使用当前包的方法，但是会执行当前包内部的init()方法， 导入可以不使用，正常导包不使用 编译会报错
	mylib2 "GolangStudy/5-init/lib2"	//别名导包，给导入的包重新起名
	//. "GolangStudy/5-init/lib2" 		//包的方法全部导入到当前包，直接调用，不需要加报名前缀， 原来 lib2.libTest()，现在直接 libTest() 调用，(不建议使用，可能会跟其他包的函数名重名)
)
``

# 10 指针
一级指针，二级指针，三指针，实际就是跳几次能找到对应的值，因为指针存的是内存地址，如果一级指针存的是还是内存地址，就需要再寻找一次，一次类推 直到找到值
``
package main
import "fmt"
func changeValue(a *int) {
	*p = 10;
}
func main(){
	var a int = 1
	changeValue(&a) //传 内存地址，由指针来接收
	fmp.Println(" a =", a) //a = 10;
}
``
# 11-defer 语句调用顺序
defer 先压Stack栈区，栈区内存 先进后出原则，   栈数据暂时存储的地方，所以才有进栈、出栈的说法。
栈区堆区 https://zhidao.baidu.com/question/2058074818739868427.html
``
package main
import "fmt"
func main(){
	//这里注意顺序
	defer fmp.Println("end 1")	//后执行
	defer fmp.Println("end 2")  //先执行
	fmp.Println("hello world")
}
``

defer 跟 return 谁先谁后， defer当前函数所有生命周期结束才出栈，所以return 要先执行，气候才是 defer
``
func returnAndDefer int () {
	defer deferFunc()	//后执行
	return returnFunc() //先执行
}
``

# 12 数组与 slice 动态数组
固定数组
``
package main
func printArray(myArray [4]int) { // 固定数组的长度，数组的长度会作为参数的条件，如果传入长度不是4的会提示 
	for index,value := rang myArray{
		fmp.Println("value = ", value)
	}
	myArray[0] = 11; // 修改后，在函数体外会不改变，固定数组 属于值拷贝
}
func main() {
	//数组定义的形式  var myArray []int  //声明
	myArray3 := [4]int{1,2,3,4} //一般直接使用这种 直接定义赋值
	printArray(myArray)
	mt.Println(" ===== 函数体内改变了固定数组的值，在函数体外打印依然没变，所以属于拷贝值传递")
	for _,value := range myArray{
		fmt.Println("value = ", value)
	}
}
``

动态数组
``
package main
func printArray(myArray []int) {
	for _,v := range myArray {
		fmt.Println("value = ", v)
	}
	myArray[0] = 100; //内部改变 外部也会变，所以动态数组切片 是引用传递
}
func main(){
	myArray : = []int{1,2,34,} //动态数组 切片 slice
	fmt.Println("myArray type is %T \n", myArray)
	printArray(myArray)
	fmt.Println(" ===== 函数体内改变了切片的，在函数体外打印已改变，证明动态数组切片传递的是 指针地址，引用传递")
	for _,value := range myArray{
		fmt.Println("value = ", value)
	}
}
``

固定数组 		传递是值拷贝传递，函数体内部改变值，函数体
动态数组(切片)传递的时间是 引用传奇，函数体内部改变值，函数外也受到影响

# 13 切片的4种声明方式

``
package main
import "fmt"
func main(){
	//1. 声明 slice1是一个切片，并且初始化，默认值是 1，2，4 长度是3
	slice1 := []int{1,2,3}
	//2. 声明 slice2是一个切片，但是没有给slice 分配空间,这个时间是不能进行赋值的，例如 slice1[0] = 10
	var slice2 []int
	slice2 = make([]int, 3) //开辟3个空间，默认值是0，这样子才能使用
	//3. 声明slice3 是一个切片，同时给 slcie 分配空间 3个空间 初始化为 0
	var slice3 []int = make([]int, 3)
	//4. 声明slice4 是一个切片，同事给slice分配3个空间，初始化为0 通过 := 推到出 是一个切片
	slice4 := make([]int, 3)
	fmt.Println("len = %d, slice = %v \n", len(slice4), slice4)
	//判断一个slice 是否为0，元素长度是否为0
	if (slcie1 == nil) {
		fmt.Println(" slice 是一个空切片")	
	} else {
		fmt.Println("slice 是有空间的")
	}
}
``

# 14
## 切片增加
切片的长度和容量不同，长度表示左指针至右指针之间的距离， 容量表示左指针至底层数组末尾的距离
切片的扩容机制，append的时间，如果长度增加后超过容量，则将容量增加2倍
``
package main
import "fmt"
func mian(){
	var numbers = make([]int, 3, 5) //扩充长度为3，容量为5
	fmt.Println("len = %d,cap = %d, slice = %v \n", len(numbers), cap(numbers), numbers)
	//向numbers 切片增加一个元素1  numbers len = 4, cap = 5
	numbers = append(numbers, 1)
	fmt.Println("len = %d,cap = %d, slice = %v \n", len(numbers), cap(numbers), numbers)
	//向numbers 切片增加一个元素2  numbers len = 5, cap = 5
	numbers = append(numbers, 2)
	fmt.Println("len = %d,cap = %d, slice = %v \n", len(numbers), cap(numbers), numbers)
	//向一个容量已满的slice 添加元素
	numbers = append(numbers, 3)
	fmt.Println("len = %d,cap = %d, slice = %v \n", len(numbers), cap(numbers), numbers)
	//如果不设置容量，容量是 之前的长度，cap 增加为之前长度的1倍 每次扩容也是扩容2倍
	fmt.Println("==============")
	var number2 = make([]int, 3) 
	fmt.Println("len = %d,cap = %d, slice = %v \n", len(number2), cap(number2), number2)
	number2 = append(number2,1)
	fmt.Println("len = %d,cap = %d, slice = %v \n", len(number2), cap(number2), number2) //此时cap变为 6，之前长度3的2倍，下一次是 6的2倍 12
}
``
## 切片截取(获取数据)
``
package main
import "fmt"
func main() {
	s := []int{1,2,3} //len = 3, cap = 3, [1,2,3]
	//取内容 原则 左开右闭
	s1 := s[0:2] //[1,2]
	fmt.Println(s1);
	//s1 跟 s 是指向同一块内存 所以 改变其中元素，两个都会变
	s1[0] = 100
	fmt.Println(s)
	fmt.Println(s1)
	//如果不想都改变，copy可以将底层数组的slice一起进行拷贝
	s2 := make([]int, 3) //s2 = [0,0,0]
	//将s中的值，一次拷贝到s2
	copy(s2, s)
	fmt.Println(s2)
}
``

# 15 map 三种声明方式
用法跟 slice 基本一致，内层数据结构不同
``
func main() {
	//====> 第一种声明方式
	//声明myMap1 是一个map类型 key是string value是string
	var myMap1 map[string]string
	if myMap1 == nil {
		fmt.Println("myMap1 是一个空map")
	}
	//在使用map钱，需要先进行 make给map分配内存空间
	myMap1 = make(map[string]string, 10)
	myMap1["one"] = "java"
	myMap1["two"] = "c++"
	myMap1["three"] = "python"
	fmt.Println(myMap1)
	//=======> 第二种声明方式
	myMap2 := make(map[int]string)  //map 长度默认可以不写， 默认会给一个值
	myMap2[1] = "java"
	myMap2[2] = "c++"
	myMap2[3] = "python"
	fmt.Println(myMap2)
	//=======> 第三种声明方式
	myMap3 := map[string]string{ "one":"php","two":"c++","three":"python"}
	// myMap3 := map[string]string{}
	fmt.Println(myMap3)
}
``
# 16 map 的使用方式
``
func printMap(cityMap map[string]string) {
	//cityMap 是一个引用传值
	fmt.Println("=========================");
	for key,value := range cityMap {
		fmt.Println("key = ", key, ", value = ", value)
	}
}
func changeValue(cityMap map[string]string) {
	//cityMap 是一个引用传值，函数体内改变，函数体外依然改变
	//如果想搞值的完全拷贝，那就直接 重新定义一个map 遍历赋值
	cityMap["England"] = "London"
}
func main() {
	cityMap := make(map[string]string)
	//添加
	cityMap["China"] = "beijing"
	cityMap["japan"] = "Tokyo"
	cityMap["USA"] = "NewYork"
	//遍历
	printMap(cityMap)
	//删除
	delete(cityMap, "China")
	//修改
	cityMap["USA"] = "NewYork1"
	//遍历
	printMap(cityMap)
	//传参map
	changeValue(cityMap)
	printMap(cityMap)
}
``
map 并发问题
https://blog.csdn.net/raoxiaoya/article/details/111469483

# 17 struct 基本定义与使用
``
type Book struct {
	name string
	auth string
}
func changeBook(book Book){
	//这里传的是拷贝副本，函数内部改变，函数外部不生效
	book.auth = "666"
}
func changeBook2(book *Book) {
	//传入的指针，指同一块内存地址，函数体内改变后，函数体外部生效
	book.auth = "777"
}
func main() {
	var book1 Book
	book1.auth = "zhang3"
	book1.name = "Golang"
	fmt.Println("%v \n", book1)
	changeBook(book1)
	fmt.Println("%v \n", book1)
	changeBook2(&book1)
	fmt.Println("%v \n", book1)
}
``
# 18 Golang 中面向对象类的表示与 封装
go中的类，实际上就是 通过结构体绑定方法来实现类对象
类名、属性名、方法名 首字母大写标识对外（其他包）可以访问，否则只能否在本包访问，这也是 fmt.Println() 中的P为什么是大写的了
``
//如果类首字母大写，表示其他包也能够访问
type Hero struct {
	//如果类的属性首字母大写，表示改属性是对外能否访问的,否则的话只能类内部访问
	Name string
	Ad 	 int
	Level int
}
/*
//this 是调用该方法的对象的一个副本(拷贝) 所以改了名字 函数体外不生效
func(this Hero) show() {
	fmt.Println("name = ", this.Name, "Ad = ", this.Ad, " Level = ", this.Level)
}
func(this Hero) getName() string {
	fmt.Println("name = ", this.Name)
	return this.Name
}
func(this Hero) setName() {
	//this 是调用该方法的对象的一个副本(拷贝) 所以改了名字  函数体外不生效
	this.Name = "newName"
}
*/
func(this *Hero) show() {
	fmt.Println("name = ", this.Name, "Ad = ", this.Ad, " Level = ", this.Level)
}
func(this *Hero) getName() string {
	fmt.Println("name = ", this.Name)
	return this.Name
}
func(this *Hero) setName(newName string) {
	//this 是调用该方法的对象的一个副本(拷贝) 所以改了名字 函数体外不能够进行
	this.Name = newName
}
func main() {
	hero := Hero{Name:"zhang3", Ad:100, Level:1}
	hero.show();
	hero.setName("li4")
	hero.show();
}
``

解释 func(this Hero) show() {}
this Hero  表示该方法绑定 Hero 结构体，属于 Hero 类的，这里 this 是调用该方法的对象的一个副本(拷贝) 所以改了名字 函数体外不生效,如果要改变 改成指针
show() 表示类方法

# 19 golang中面向对象中的 继承
``
type Human struct{
	name string
	sex string
}
func(this *Human) Eat(){
	fmt.Println("Human.Eat()")
}
func(this *Human) Walk(){
	fmt.Println("Human.Walk()")
}
//======= 子类 superman 继承
type SuperMan struct{
	Human	//SuperMan类继承l Human类的方法
	level int
}
//重新定义类的方法Eat()
func (this *SuperMan) Eat() {
	fmt.Println("SuperMan.Eat()")
}
//子类的新方法
func (this *SuperMan) Fly() {
	fmt.Println("SuperMan.Fly()")
}
func main() {
	h := Human{"zhang3", "nan"}
	fmt.Println(h)
	//定义一个子类对象
	//1.直接定义并初始化，就是Human要出现，感觉跟两个组合似的，实际继承后属于一个类的，也可以用方法二进行定义
	s := SuperMan{Human{"li4", "nv"}, 1}
	//2.直接定义子类的格式，然后可以调用父类的属性
	// var s SuperMan
	// s.name = "l4"
	// s.sex = "nv"
	// s.level = 2
	s.Walk() //父类的方法
	s.Eat() //子类重新定义的方法
	s.Fly() //子类新加的方法
	fmt.Println(s)
}
``

# 20 golang 面向对象中的  多态的实现 与 基本要素
golang中的多台要借助 接口 interface 来实现， 设计模式中 例如 工厂、代理 等离不开多态

基本要素
1.有一个父类（有接口）c++中实现虚函数或纯虚函数
2.有子类（实现了父类的全部接口方法）
3.父类 类型的变量(指针)指向(引用)子类的具体 数据变量
``
//本质是一个指针
type AnimalIF interface {
	Sleep()
	GetColor() string
	GetType() string
}
//只要类中把接口中的方法实现了就行，不用再类中再次 写AnimalIF 继承 进行继承
type Cat struct {
	color string
}
func (this *Cat) Sleep() {
	fmt.Println("cat is sleep")
}
func (this *Cat) GetColor() string{
	return this.color
}
func (this *Cat) GetType() string{
	return "Cat"
}
//=======Dog 类
type Dog struct {
	color string
}
func (this *Dog) Sleep() {
	fmt.Println("Dog is sleep")
}
func (this *Dog) GetColor() string {
	return this.color
}
func (this *Dog) GetType() string {
	return "Dog"
}
func showAnimal(animal AnimalIF) {
	animal.Sleep();
}
func main() {
	var animal AnimalIF //接口的数据类型， 实际就是一个父类指针
	animal = &Cat{"Green"} 
	animal.Sleep() //调用的就是 Cat的 Slee()方法, 多态现象
	animal = &Dog{"Yellow"} 3.父类 类型的变量(指针)指向(引用)子类的具体 数据变量
	animal.Sleep();
	//另外一种形式
	cat := Cat{"Green"}
	showAnimal(&cat)
	dog := Dog{"Dog"}
	showAnimal(&dog)
}
``
# 21 interface 空接口万能类型 与 类型断言机制
``
//interface 是万能数据类型 
func myFunc(arg interface{}) {
	fmt.Println("myFunc is called ....")
	fmt.Println(arg);
	//interface 是万能数据类型，哪如何区分类型是什么？
	// 给 interface 提供了 ”类型断言“的机制
	value, ok := arg.(string)
	if !ok {
		fmt.Println("arg is not string type")
	} else {
		fmt.Println("arg is string type")
		fmt.Printf("value type is %T\n", value)  //注意这里是 Printf， 不是 Println
	}
}
func main() {
	myFunc(1)
	myFunc(3.14)
	myFunc("name")
}
``

# 22 变量的内置 pair 结构详细说明
断言能够成功，取决于go的变量构造， 变量结构 包含 type value 的pair对
type
	static type  (int, string 等)
	concrete type (interface 所指向的具体数据类型，系统看的见的类型)
value

``
func main() {
	var a string;
	a  = "aceId"
	//pair<statictype:string, value:"aceId">
	//没一个变量 内部都有一个pair, 传递的时间同时将这个pair 传递
	//pair<type:string, value:"aceId"
	var allType interface{}
	allType = a
	str,_ := allType.(string)
	fmt.Println(str)
}
``

# 23 反射 reflect 机制用法
原理 反射通过变量获取pair中具体的type value
简单案例
``
package main
import (
	"fmt"
	"reflect"
)
func reflectNum(arg interface{}) {
	fmt.Println("type = ", reflect.TypeOf(arg))
	fmt.Println("value = ", reflect.ValueOf(arg))
}
func main() {
	var num float64 = 1.23
	reflectNum(num)
}
``
复杂结构的案例
``
package main
import (
	"fmt"
	"reflect"
)
type User struct {
	Id   int
	Name string
	Age  int
}
func (this User) Call() { //注意这个地方 没传指针，当时这个地方传指针，导致reflect.NumMethod 获取到是的0, 如果是指针哪传入的时间就传入 &user
	fmt.Println("user is calded ..")
	fmt.Printf("%v\n", this)
}
func main() {
	user := User{1, "aceId", 18}
	DoFiledAndMethod(user)
}
func DoFiledAndMethod(input interface{}) {
	//获取 input的 type
	inputType := reflect.TypeOf(input)
	fmt.Println("inputType is ", inputType)
	//获取 input的 value
	inputValue := reflect.ValueOf(input)
	fmt.Println("inputValue is ", inputValue)
	//通过type 获取里边的字段
	//1.获取 interface 的reflect.Type 通过Type得到 NumField，进行遍历
	//2.获得多个field，数据
	//3.通过field有一个 Interface() 方法获得 对应的value
	for i := 0; i < inputType.NumField(); i++ {
		field := inputType.Field(i)
		value := inputValue.Field(i).Interface()
		fmt.Printf("%s: %v = %v\n", field.Name, field.Type, value)
	}
	//通过type 获取里边的方法，调用
	fmt.Println("=========", inputType.NumMethod()) //如果传指针，这个地方获取的是零， 你如果要看指针类型的方法，TypeOf函数传进去指针就好
	for i := 0; i < inputType.NumMethod(); i++ {
		m := inputType.Method(i)
		fmt.Println(m)
		fmt.Printf("%s: %v\n", m.Name, m.Type)
	}
}
``

# 24 反射 解析结构体标签Tag
结构体标签Tag 使用 反射才能解读
go 允许在结构体语法加标签，标签的作用给别的包调用的时间提供一个说明
``
package main
import (
	"fmt"
	"reflect"
)
type resume struct {
	Name string `info:"name" doc:"doc name"`
	Sex  string `info:"sex"`
}
func findTag(str interface{}) {
	t := reflect.TypeOf(str).Elem()
	for i := 0; i < t.NumField(); i++ {
		taginfo := t.Field(i).Tag.Get("info")
		tagdoc := t.Field(i).Tag.Get("doc")
		fmt.Println("info", taginfo, "doc", tagdoc)
	}
}
func main() {
	var re resume
	findTag(&re)
}
``

# 25 结构体标签在 json中的应用
结构体标签的应用： 解析json 、 orm 等需要
``
package main
import (
	"encoding/json" //编码包
	"fmt"
)
type Move struct {
	Name   string   `json:"title`  //这里是json格式的  字段名 title 后续 编码成 json的时间 需要
	Year   int      `json:"year`
	Price  int      `json:"rmb"`
	Actors []string `json:"actor"`
}
func main() {
	movie := Move{"喜剧之王", 2000, 10, []string{"xingye", "bozhi"}}
	//编码过程 结构体-> json
	jsonStr, err := json.Marshal(movie)
	if err != nil {
		fmt.Println("json marshal error", err)
		return
	}
	fmt.Printf("jsonStr = %s\n", jsonStr)
	//解码过程 jsonstr -> 结构体
	//jsonStr = {"Name":"喜剧之王","Year":2000,"rmb":10,"actor":["xingye","bozhi"]}
	myMove := Move{}
	err = json.Unmarshal(jsonStr, &myMove)
	if err != nil {
		fmt.Println("json marshal error", err)
		return
	}
	fmt.Println(myMove)
}
``
# 26 goroutine 基本模型 和调度设计策略

* 单进程操作系统 
(每一个进程顺序执行的 执行线程也可以，cpu 无法区别  进程跟线程的，尤其对 linux 系统， 同一时刻 只能执行一个进程)
1.单一执行流程、计算机只能一个任务一个任务处理
2.进程阻塞 所带来的CPU浪费

* 多线程/多进程操作系统
 (宏观的执行多个任务) 轮询调度器 实现并发执行，不是并行执行
CPU调度器，执行3个进程，做一个时间片的切分 每个进程允许执行的不能超过时间片的时间，超过执行下一个进程，知道轮询一遍再回来执行

* 多进程/多线程 解决了阻塞问题，面临新的问题
1.进程切换成本（切换 就有拷贝切换），进程/线程的数量越多，切换成本就越大，也浪费， 可能60%在执行程序 40%在处理切换，cpu利用率也就6 0%，利用率不高，这种模式下只能从软件设计上提高cpu利用率
2.多线程 随着同步竞争（如锁、竞争资源冲突等） 开发设计 变得越来越复杂
弊端 高消耗调度CPU， 高内存占用，（不停的在进程切换，进程资源独立 虚拟内存 4GB[32bit Os]， 线程约占 4MB）

需要解决
一个线程 分为  用户空间[用户线程], 内核空间[内核线程]，两个空间进行 绑定
对于CPU 无感，只能看到一个内核线程(thread)，而且这个内核线程 绑定 用户线程(携程 co-routine)  无须切换，解决了CPU高消耗调度的问题
N:1 用户态 多个携程 通过 携程调度器(轮询) 与一个内核线程进行绑定，遇到的问题携程一个阻塞会影响下一个携程的执行
1:1 携程的创建、删除和切换的代建都是由CPU完成有点略显昂贵了
M:N 多个携程 通过 携程调度器 绑定 多个 线程，瓶颈再携程调度器

Golang对携程的处理 co-routine -> Goroutine, 改内存几KB， 灵活调度(go调度器的作用)
符号含义  G-goroutine携程， M-threas线程
全局go协程队列， M0调用 全局go队列中拿一个G协程处理，结束后还锁，在继续下一个 M其他线程需要等待锁
GO老调度器缺点
1.创建、销毁、调度G都需要每个M获取锁，这就形成了 激烈的锁竞争
2.M转移G会造成延迟和额外的系统负载
3.系统调用(CPU在M之间的切换) 导致频繁的线程阻塞和取消塞操作阻塞操作 增加了系统开销

GMP（goroutine携程，processor处理器， thread线程）

图

调度器的设计策略
复用线程	  
并行执行： GOMAXPROCS 限定P的个数 例如 CPU核数/2
抢占，     如果没有可用cpu，每个G 处理10ms后就会别的G来抢这个线程
全局G队列  work Stelling 偷取的时间，优先从每个P的队列单独处理，如果没有在去G队列进行 

# 27 创建 goroutine
``
//子 goroutine
func newTask() {
	i := 0
	for {
		i++
		fmt.Println("new Goroutin i = %d", 1)
		time.Sleep(1 * time.Second)
	}
}
func main() {
	go newTask() //有名函数调用
	fmt.Println("main goroutine exit")
	go func() {
		defer fmt.Println("A.defer")
		func() {
			defer fmt.Println("B.defer")
			runtime.Goexit() //退出当前的 goroutine
			fmt.Println("B")
		}() //匿名函数小括号调用 ()
		fmt.Println("A")
	}()
	//匿名函数传值
	go func(a int, b int) {
		fmt.Println("a = ", a, "b = ", b)
	}(100, 200)
	for {
		time.Sleep(1 * time.Second)
	}
}
``
# 28 channel 的基本定义与使用
goroutine之间相互通信的数据类型 channel 管道
channel 自身有同步两个goroutine的功能
``
func main() {
	//定义一个channel
	c := make(chan int)
	go func() {
		defer fmt.Println("goroutine 结束")
		fmt.Println("goroutine 开始")
		c <- 666 //思考下 如果这个地方是需要塞入多个值，外边接收如何处理，引出下一个点的有缓存channel
	}()
	num := <-c
	fmt.Println("num = ", num)
	fmt.Println("main goroutine end")
}
``
分析 
main goroutine 在读取chanel 数据 没有的情况下 会阻塞等待知道收到数据
sub  goroutine 在写入如果没有接受的也会等待
所以 无论 main先执行，还是 sub中的 goroutine先执行到，整体还是是一个同步的流程

# 29 有缓冲与无缓冲同步问题
* 使用无缓冲的通道在 goroutine 之间同步
1.在两个goroutine都达到通道，但那个都没有开始执行发送后者接收
2.在左侧goroutine 向通道发送数据，此时这个goroutine  会在通道中被锁住，直到交换完成
3.在右侧goroutine 从通道接收数据，此时这个goroutine也会在通道中被锁住，直到交换完成
4.交换完成 两个锁住的goroutine 得到的释放。两个goroutine现在都可以去做其他事了

* 使用有缓冲的通道在 goroutine 之间同步数据
1.右侧的goroutine正在从通道接收一个值
2.右侧的goroutine独立完成接收的动作，而左侧的goroutine 正在发送一个新值到通道里
3.左侧的goroutine还在向通道接收值的动作，而右侧的goroutine正在从通道接收另外一个值。两个操作即不是同步，也不会相互阻塞(当空间不够的时间会锁)
4.所有的发送和接收完成，而通道里还有几个值，也有一些空间可以存更多的值
特点 
当channel已满，再向里面写数据，就会阻塞
当channel为空，从  里边取数据，也会阻塞
前提都是管道没有关闭

``
func main() {
	c := make(chan int, 3)
	fmt.Println("len(c) = ", len(c), "cap(c) = ", cap(c))
	go func() {
		defer fmt.Println("子goroutine 结束")
		 //i改成1，那就塞入3个元素，main中读取四次，会导致锁住，而当前goroutine已结束不会再发数据，所以会产生死锁，引入下一个带你 channel close
		for i := 0; i < 4; i++ {
			c <- i
			fmt.Println("子goroutine 发送元素=", i, "len(c) = ", len(c), "cap(c)= ", cap(c))
		}
	}()
	time.Sleep(2 * time.Second)
	for i := 0; i < 4; i++ {
		num := <-c
		fmt.Println("num = ", num)
	}
	fmt.Println("main 结束")
}
``

# 30 channel的关闭特点
channel 不像文件一样需要经常关闭，只有当你确定没有任何数据发送了，或者你想显式的结束range循环之类的 才去关闭channel
关闭channel后，无法向channel再发送数据（引发 panic 错误后导致接受立即返回零值）
关闭channel后，可以继续从channel接收数据
对于 nil channel，无论收发都会阻塞
``
func main() {
	c := make(chan int)
	go func() {
		for i := 0; i < 5; i++ {
			c <- i
		}
		//close 可以关闭一个 channel
		close(c)
	}()
	for {
		//ok为true 标识 channel 没有关闭，如果为false标识 channel已经关闭
		if data, ok := <-c; ok { // 先执行分好前的表达式，后边的ok 相当于if的局部形参，if最终判断是后边的ok
			fmt.Println("data =", data)
		} else {
			break
		}
	}
	fmt.Println("main Finished..")
}
``
# 31 channel 与 range
主要从channel读取数据 for data := range c {}
``
func main() {
	c := make(chan int)
	go func() {
		for i := 0; i < 5; i++ {
			c <- i
		}
		//close 可以关闭一个 channel
		close(c)
	}()
	for {
		//ok为true 标识 channel 没有关闭，如果为false标识 channel已经关闭
		if data, ok := <-c; ok { // 先执行分好前的表达式，后边的ok 相当于if的局部形参，if最终判断是后边的ok
			fmt.Println("data =", data)
		} else {
			break
		}
	}
	//重点 上边的可以简写成 
	for data := range c {
		fmt.Println("data = ", data)
	}
	fmt.Println("main Finished..")
}
``
# 32 channel 与 select
单流程下 一个map只能监控一个channel的状态，select可以完成监控多个channel的状态
seelct 具备多路channel的监控状态功能
``
func fibonacii(c, quit chan int) {
	x, y := 1, 1
	for {
		select {
		case c <- x:
			//如果c可写，则case进来
			x = y
			y = x + y
		case <-quit:
			fmt.Println("quit")
			return
		}
	}
}
func main() {
	c := make(chan int)
	quit := make(chan int)
	//sub go
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	// main go
	fibonacii(c, quit)
}
``
# 34 GO PATH工作模式的弊端
弊端
没有版本管理， 		go get -u github.com/aceid/zindex  这个没法指定版本，默认下载最新
无法同步一致第三库
无法指定当前项目引用的第三方版本号，本地装什么什么版本，导包就导的什么版本

# 35 GoModules 模式基础环境说明
特殊环境变量
1.GO111MODULE 是否开启go modules 模式
go env -w GO111MODELE="on"
2.GOPROXY  设置下载代理，direct 用于指示go 回源到模块版本的源地址去抓取（比如 github等）
go env -w GOPROXY="https://goproxy.cn,direct"
3.GOSUMDB 校验下载包的完整性没有被篡改过的，通过设置的是网址进行校验 ，设置了GOPROXY 校验也会通过这个地址
4.私有仓库 GONOPROXY/GONOSUMDB/GOPRIVATE, 设置一个GOPRIVATE就行覆盖
go env -w GOPRRIVATE="git.exameple.com,github.com/aceid/zinx"

# 35 go modules 初始化项目
一、开启GO MODULES 模块
go env -w GO111MODELE="on"
export GO111MODULE=one
二、初始化项目
1.任意文件创建一个项目
2.创建go.mod 文件 go mod init github.com/aceid
3.会生成一个 go.sum 文件
4.在该项目编写源码依赖某个库
手动下载 go get github.com/aceid/zinx （会下载到 GOPATH/src/mod/）
go.mod 会添加一行代码，解析
indiret 间接依赖
h1:哈希值（下载包的所有文件做的一个哈希值），如果不存在，标识依赖的库可能用不上
go.mod:哈希值（当前go.mod文件的哈希值）

生成一个 go.sum 文件作用 罗列当前项目直接或者间接所有模块版本，保证今后依赖的版本不会被篡改

# 36 改变模块依赖关系
go mod edit -repalce=zinx@v0.1=zinx@v0.2

go.mod 文件会新增一行  replace zinx v1 => v2


同一个目录下面不能有个多 package main
创建map映射表和slice切片的时候都必须用make，或者初始化，否则会报编译错误：“not an expression”，（不是表达式）

