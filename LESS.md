# LESS

## 1 语法

### (1) 注释

> //注释内容		转化后消失
>
> /* *注释内容* */		转化后保留



### (2) 变量声明及使用

```less
//变量的声明
@变量名 : 值;//一般定义形式

@变量名 : ~'带特殊符号的字符串';//变量值中存在特殊字符 

//变量的使用
@变量名;// 一般变量使用
@{变量名};// 变量值带特殊符号的变量
```



#### less变量的作用域和延迟加载

延迟加载： 

​	less的变量作用流程如下：

​	1、先获取声明的变量与变量值，将其全部前置

​	2、以作用域的思维进行变量的引用



### (3)  less混合

#### 不带参数的混合

```less
.center-box01() {    
  width: 400px;    
  height: 300px;    
  float: left;    
  margin: 10px;
}
//使用
.news {    
  .center-box01();
}.box {    
  .center-box01();
}
```



#### 带参数的混合

```less
//声明
.div-box(@w,@h,@bg){
  width:@w;
  height:@h;
  background:@bg;
}

//使用
.box{
  .div-box{@bg:#fff,@h:200px,@w:380}
}
//使用的时候，传参顺序不影响使用
```



#### 带默认值的混合

```less
//声明
.div-box(@w:300px,@h:200px,@bg:#fff){
	width:@w;
  	height:@h;
  	background:@bg;
}

//使用
.box{
  //传递部分参数 传参对应属性替换，其余使用默认值
  .div-box(@bg:transparent)
}
```



#### @arguments

```less
//@arguments在混合中使用，可以获取所有实参序列
.div-box(@property,@time,@ease:ease-in){
  width:400px;
  height:400px;
  border:1px solid #ddd;
  -webkit-transition: @arguments;
  	 -moz-transition: @arguments;
  		  transition:@arguments;
}

//使用
.box{
  .div-box(all,.5s,ease-out)
}

//使用2
.box2{
  .div-box(width,.8s)
}
```



### (4) 条件判断

**when**

```less
//定义带有条件的混合
.bd-color(@w,@color,@direction) when (@direction = top){
  border-width:@w;
  border-color: @color transparent transparent transparent;
}
.bd-color(@w,@color,@direction) when (@direction = right){
  border-width:@w;
  border-color:transparent @color transparent transparent;
}
.bd-color(@w,@color,@direction) when (@direction = bottom){
  border-width:@w;
  border-color:transparent transparent @color transparent;
}
.bd-color(@w,@color,@direction) when (@direction = left){
  border-width:@w;
  border-color:transparent transparent transparent @color;
}
.bd-color(@w,@color,@direction){
  width:400px;
  height:400px;
  background:#ddd;
  border-style:soild;
  border-width:@w;
}

//使用
.box{
  .bd-color(2px,top,#ddd);
}
```



### (5) less导入

* 可以导入一个.less文件，这样导入less文件中包含的变量可以进行使用
* 如果导入的文件扩展名为`.less` ，则可以将扩展名省略

```less
//导入less文件
/*
	@import 'less文件的地址';
*/
@import 'index.less';
@import 'index';

//导入css文件
/*
	@import 'css文件的地址'
*/
@import 'index.css'
```



### (6) less嵌套

#### 基本使用

```less
#header{
  color:black;
  .navigation{
    font-size:12px;
    li{
      width:200px;
    }
  }
  .logo{
    width:300px;
  }
}
```



```css
#header{
  color:black;
}
#header .navigation{
  font-size:12px;
}
#header .navigation li{
  width:200px;
}
#header .logo{
  width:300px;
}
```



#### &符号应用

​	&：上一层选择器的名字

```less
// & 符号避免产生层级结构，用于交集选择器、伪类选择器、属性选择器、伪元素选择器
.item{
  &.active{}
  &:hover{}
  &::before{}
  &[title]{}
}
//上面代码依次实现
.item.active{}
.item.hover{}
.item::before{}
.item[title]{}
```



#### 媒体查询的嵌套

```less
.container{
	margin: 0 auto;
	width:100%;
	@media(min-width:768px){
		width:750px;
	}
	@media(min-width:992px){
		width:970px;
	}
	@media(min-width:1200px){
		width:1170px;
	}
}
```

编译后：

```css
.container{
  margin: 0 auto;
  width:100%;
}
@media (min-width:768px){
  .container{
    width:750px;
  }
}
@media (min-width:992px){
  .container{
    width:970px;
  }
}
@media (min-width:1200px){
  .container{
    width:1170px;
  }
}
```



#### 混合和嵌套结合

```less
.clearfix(){
  &::after{
    content:'';
    display:block;
    clear:both;
  }
}
div{
  .clearfix();
}
```

编译后：

```css
div::after{
  	content:'';
    display:block;
    clear:both;
}
```



### (7) less运算符

 运算符需要两个操作数，操作数的单位对于计算结果有影响：

   ① 如果两个操作数都没有单位，结果也没有单位

   ② 如果只有一个操作数有单位，结果使用这个单位

   ③ 如果两个操作数都有单位且不一样，结果使用第一个操作数的单位

```less
width: 100px+20;    // width: 120px;
z-index: 1000-200;  // z-index: 800;
width: 100px+200px; // width:300px;
width: 100px+200vw; // width:300px;
width: (400px/2);   // width: 200px;
```



###(9) less函数

* 判断类型

| 函数         | 说明             |      |
| ---------- | -------------- | ---- |
| isnumber() | 判断给定的值是否是一个数字  |      |
| iscolor()  | 判断给定的值是否是一个数字  |      |
| isurl()    | 判断给定的值是否是一个url |      |

* 颜色操作

| 函数       | 说明            |
| -------- | ------------- |
| saturate | 增加一定数值的颜色饱和度  |
| lighten  | 增加一定数值的颜色亮度   |
| darken   | 降低一定数值的透明度    |
| fade     | 给颜色设定一定数值的透明度 |
| mix      | 根据比例混合两种颜色    |

* 数学函数

| 函数                | 说明            |
| ----------------- | ------------- |
| celi(`num`)       | 向上取整          |
| floor(`num`)      | 向下取整          |
| percentage(`浮点数`) | 将浮点数转换为百分比字符串 |
| round(`num`)      | 四舍五入          |
| sqrt(`num`)       | 计算一个数的平方根     |
| abs(`num`)        | 计算数字的绝对值      |
| pow(m,n)          | 计算一个数的幂 m^n^  |

