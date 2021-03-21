# 1. 基本语法

1.JS中严格区分大小写
2.JS中会忽略多个空格和换行，所以我们可以利用空格和换行对代码进行格式化

# 2. 标识符

JS中所有的可以由我们自主命名的都可以称为是标识符
例如变量名，函数名，属性名都属于标识符
标识符命名规则：
1.标识符中可以含有字母、数字、_、$
2.标识符不能以数字开头
3.标识符不能是ES中的关键字或保留字
4.标识符一般都采用驼峰命名法

# 3. 数据类型

在JS中一共有6种数据类型(ES5)
    String 字符串
    Number 数值
    Boolean 布尔值
    Null 空值
    Undefined 未定义
    Object 对象

其中 String Number Boolean Null Undefined 属于基本数据类型
而Object属于引用数据类型

## 3.1. String

String字符串
    在JS中字符串需要使用引号
    使用双引号或单引号都可以，但不能混着用
    引号不能嵌套，双引号里不能放双引号，单引号里不能放单引号

在字符串中我们可以使用 \ 作为转义字符，当表示一些特殊符号时可以使用\进行转义

```
    \" 表示 "
    \' 表示 '
    \n 表示换行
    \t 制表符
    \\ 表示\
```

## 3.2. Number

在JS中所有的数值都是Number类型，包括整数和浮点数

JS中可以表示的数字的最大值
Number.MAX_VALUE

JS中可以表示的大于0的最小值
Number.MIN_VALUE

如果使用Number表示的数字超过了最大值，则会返回
    Infinity 正无穷
    -Infinity 负无穷
    使用typeof检查Infinity也会返回number

NaN是一个特殊的数字，表示Not A Number
    使用typeof检查一个NaN也会返回number

注意：如果使用JS进行浮点运算，可能得到一个不精确的结果 如：0.1+0.2
所以千万不要使用JS进行对精确度要求比较高的运算

## 3.3. Boolean

Boolean布尔值
布尔值只有两个，主要用来做逻辑判断
true：表示真
false：表示假
使用typeof检查一个布尔值时，会返回boolean

## 3.4. Null

Null（空值）类型的值只有一个，就是null
null这个值专门用来表示一个为空的对象

注意:使用typeof检查一个null值时，会返回object

## 3.5. Undefined

Undefined（未定义）类型的值只有一个，就undefind
当声明一个变量，但是并不给变量赋值时，它的值就是undefined

# 4. 强制类型转换

类型转换主要指，将其他的数据类型，转换为
String Number Boolean

## 4.1. 数据类型转换为String

方法一：
    调用被转换数据的toString()方法
    该方法不会影响原变量，它会将转换的结果返回
    注意：null和undefined这两个值没有toString()方法，如果调用会报错
```
    var a = 123
    console.log(a.toString())
```

方法二：
    调用String()函数，并将被转换的数据作为参数传递给函数
    使用String()函数做强制类型转换时
    对于Number和Boolean实际上就是调用toString()方法
    但是对于null和undefined，不会调用toString()方法
    它会将null直接转换为"null"
    将number直接转换为"number"
```
    var a = 123
    console.log(String(a))
```

## 4.2. 数据类型转换为Number

方法一：
    使用Number()函数

   字符串-->数字
    1.如果是纯数字的字符串，则直接将其转换为数字
    2.如果字符串中有非数字的内容，则转换为NaN
    3.如果字符串是一个空串或者是一个全是空格的字符串，则转换为0

   布尔值-->数字
   true转成1
   false转成0

   null-->数字
   转为0

   undefined-->数字
   转为NaN

方法二：
    这种方法专门应对字符串
    parseInt() 把一个字符串转换为一个整数
    parseInt() 可以将一个字符串中的有效的整数内容取出来，然后转换为Number
    parseFloat() 把一个字符串转换为一个浮点数
    如果对非字符串使用parseInt()或parseFloat()
    它会先将其转换为String然后再操作
    注意：首个元素必须是数字，否则NaN

   在JS中
   16进制，0x开头
   8进制，0开头
   2进制，0b开头

   可以在parseInt()中传递第二个参数，用来指定数字的进制
   `a = parseInt(a , 10);`

## 4.3. 数据类型转换为Boolean

使用Boolean()函数
    数字-->布尔值
    除了0和NaN，其余都为true

   字符串-->布尔值
   除了空串，其余都为true

   null和undefined都会转换为false
   对象也会转换为true

# 5. 运算符/操作符

通过运算符可以对一个或多个值进行运算,并获取运算结果
比如：typeof就是运算符，可以来获得一个值的类型，它会将该值的类型以字符串的形式返回

## 5.1. 算数运算符

当对非Number类型的值进行运算时，会将这些值转换为Number然后再运算，任何值和NaN做运算都得NaN

"+"
可以对两个值进行加法运算，并将结果返回
如果对两个字符串进行加法运算，则会做拼串：将两个字符串拼接为一个字符串，并返回

任何的值和字符串做加法运算，都会先转换为字符串，然后再和字符串做拼串的操作

我们可以利用这一特点，来将一个任意的数据类型转换为String
我们只需要为任意的数据类型 + 一个 "" 即可将其转换为String
这是一种隐式的类型转换，由浏览器自动完成，实际上它也是调用String()函数

"-"
可以对两个值进行减法运算，并将结果返回

"*"
可以对两个值进行乘法运算

"/"
可以对两个值进行除法运算

"%"
取模运算（取余数）

任何值做- * /运算时都会自动转换为Number

我们可以利用这一特点做隐式的类型转换
可以通过为一个值 -0 *1 /1来将其转换为Number

原理和Number()函数一样，使用起来更加简单

## 5.2. 一元运算符

一元运算符，只需要一个操作数

"+"
正号不会对数字产生任何影响

"-"
负号可以对数字进行负号的取反
对于非Number类型的值，它会将先转换为Number，然后在运算，可以对一个其他的数据类型使用+,来将其转换为number
它的原理和Number()函数一样

自增"++"
通过自增可以使变量在自身的基础上增加1
对于一个变量自增以后，原变量的值会立即自增1
自增分成两种：后++(a++) 和 前++(++a)

无论是 a++ 还是 ++a，都会立即使原变量的值自增1
不同的是a++ 和 ++a的值不同
a++的值等于原变量的值（自增前的值，因为加号在后面）
++a的值等于新值 （自增后的值，因为加号在前面）

自减"--"
通过自减可以使变量在自身的基础上减1
自减分成两种：后--(a--) 和 前--(--a)
无论是a-- 还是 --a 都会立即使原变量的值自减1
不同的是a-- 和 --a的值不同
a-- 是变量的原值 （自减前的值，因为减号在后面）
--a 是变量的新值 （自减以后的值，因为减号在前面）

## 5.3. 逻辑运算符

! 非
!可以用来对一个值进行非运算
所谓非运算就是值对一个布尔值进行取反操作，true变false，false变true
如果对一个值进行两次取反，它不会变化
如果对非布尔值进行元素，则会将其转换为布尔值，然后再取反
所以我们可以利用该特点，来将一个其他的数据类型转换为布尔值
可以为一个任意数据类型取两次反，来将其转换为布尔值，原理和Boolean()函数一样

&& 与
&&可以对符号两侧的值进行与运算并返回结果
运算规则：
只有两个值都为true时，才会返回true
JS中的“与”属于短路的与（如果第一个值为false，则不会看第二个值）
简单来说就是找false

|| 或
||可以对符号两侧的值进行或运算并返回结果
两个值中只要有一个true，就返回true
JS中的“或”属于短路的或（如果第一个值为true，则不会检查第二个值）
简单来说就是找true

&& || 非布尔值的情况
对于非布尔值进行与或运算时，会先将其转换为布尔值，然后再运算，并且返回原值

与运算：如果第一个值为true，则必然返回第二个值
如果第一个值为false，则直接返回第一个值

或运算:如果第一个值为true，则直接返回第一个值
如果第一个值为false，则返回第二个值

## 5.4. 关系运算符

通过关系运算符可以比较两个值之间的大小关系，如果关系成立它会返回true，如果关系不成立则返回false

">"大于号
判断符号左侧的值是否大于右侧的值
如果关系成立，返回true，如果关系不成立则返回false

">=" 大于等于

"<" 小于号

"<=" 小于等于

"=="相等
当使用==来比较两个值时，如果值的类型不同，则会自动进行类型转换，将其转换为相同的类型然后再比较

"!="不相等
不相等用来判断两个值是否不相等，如果不相等返回true，否则返回false
不相等也会对变量进行自动的类型转换，如果转换后相等它也会返回false

非数值的情况
对于非数值进行比较时，会将其转换为数字然后在比较
如果符号两侧的值都是字符串时，不会将其转换为数字进行比较,而会分别比较字符串中字符的Unicode编码
比较两个字符串时，比较的是字符串的字符编码,比较字符编码时是一位一位进行比较,如果两位一样，则比较下一位，所以借用它来对英文进行排序
但比较中文时没有意义

任何值和NaN做任何比较都是false，如果比较的两个字符串型的数字，可能会得到不可预期的结果，所以在比较两个字符串型的数字时，一定一定一定要转型

## 5.5. 条件运算符/三元运算符

语法：
    条件表达式?语句1:语句2;

执行的流程：
    条件运算符在执行时，首先对条件表达式进行求值
    如果该值为true，则执行语句1，并返回执行结果
    如果该值为false，则执行语句2，并返回执行结果
    如果条件的表达式的求值结果是一个非布尔值，会将其转换为布尔值然后在运算
    不推荐使用

## 5.6. 运算符的优先级

就和数学中一样，在JS中运算符也有优先级
比如先乘除后加减
JS中有运算符优先表，在表中越靠上优先级越高，优先级越高越优先计算
但这个表不需要记忆，如果遇到优先级不清楚可以使用（）来改变优先级

## 5.7. 详解隐式转换

转换布尔类型为false：NaN  null  undefined  +0  -0  ''  document.all

转换为Number为0：false  ''  null  (注意undefined转换为NaN

转化为String，注意null,undefined不是调用toString()方法，而是直接返回'null' 'undefined'

判断的基本规则：
对象 == 数字或字符串
toPrimitive(对象) ： 对象.valueOf().toString()

布尔值 == 任何类型
Number(布尔值)：将布尔值转化为数字

数字 == 字符串
Number(字符串)：将字符串转化为数字

undefined == null
true



例题：
console.log(1 + 'true'); // 1true
console.log(1 + true); // 2
console.log(1 + undefined); // NaN
console.log(1 + null); // 1
console.log('2' > 10); // false
console.log('2' > '10'); // true 依次比的是unicode编码
console.log('abc' > 'aad'); // true
console.log(NaN == NaN); // false NaN和任何值比都是false
console.log(undefined = null); // true
console.log([1,2] == '1,2') // true 数组的valueOf().toString()方法转字符串
console.log([] == 0) // true 数组的valueOf().toString()空串转数字为0
consolo.log(![] == 0) // true []经过Boolean()为true取反为false
console.log({} == !{}) // false {}布尔为true取反为false，Number后为0 {}valueOf().toString()为'[object Object]'Number()后为NaN 
console.log({} == {}) // false 引用不同
console.log([] == []) // false 引用不同
console.log('true' == true) // false，true转数字为1  'true'转数字为NaN

经典题
const a = ???
if (a == 1 && a == 12 && a == 23) {
    console.log(true)
}
引用类型隐式转换为原始类型流程：
调用toPrimitive()内部函数
1.是否为原始类型？是则返回
 2.否则调用valueOf()方法，返回值是否为原始类型？是则返回
3.否则调用toString()方法，返回值是否为原始类型？是则返回
4.否则，抛出错误
所以我们可以通过定义引用对象的valueOf()或toString()方法来覆盖Object.prototype的方法

const a = {
    value: -10,
    valueOf() {
        return this.value += 11
    }
}
if (a == 1 && a == 12 && a == 23) {
    console.log(true) 
}

# 6. 编码

在字符串中使用转义字符输入Unicode编码
即：\u四位编码

在html中使用Unicode编码
&#x编码

# 7. 流程控制语句

## 7.1. if语句

语法一：

```
if (条件表达式) {
    语句...;
}
```

语法二：

```
if (条件表达式) {
    语句...;
}
else {
    语句...;
}
```

语法三

```
if (条件表达式) {
    语句...;
}
else if (条件表达式) {
    语句...;
}
else if (条件表达式) {
    语句...;
}
else {
    语句...;
}

```

## 7.2. switch语句

语法：

```
switch (条件表达式) {
    case 表达式:语句...;break;
    case 表达式:语句...;break;
    case 表达式:语句...;break;
    default:语句...;break;
}
```

## 7.3. while语句

语法一：

```
while (条件表达式) {
    语句...;
}
```

语法二：

```
do {
    语句...;
} while (条件表达式);
```

## 7.4. for语句

语法：

```
for(①初始化表达式; ②条件表达式; ④更新表达式) {
    ③语句...;
}
```

# 8. 对象

JS中数据类型
String 字符串
Number 数值
Boolean 布尔值
Null 空值
Undefined 未定义
以上这五种类型属于基本数据类型，只要不是上边的5种，全都是 Object 对象

## 8.1. 对象的分类

1.内建对象
    由ES标准中定义的对象，在任何的ES的实现中都可以使用，比如：Math String Number Boolean Function Object....

2.宿主对象
    由JS的运行环境提供的对象，目前来讲主要指由浏览器提供的对象，比如 BOM DOM

3.自定义对象
由开发人员自己创建的对象

## 8.2. 创建对象方法

自定义对象创建方法
var obj = new Object();
var obj2 = {
    name:"猪八戒",
    age:13,
    gender:"男",
    test:{name:"沙僧"}
};

向对象添加属性
语法：对象.属性名 = 属性值;
obj.name = "孙悟空";
obj.gender = "男";
obj.age = 18;

语法：对象["属性名"] = 属性值
使用[]这种形式去操作属性，更加的灵活，在[]中可以直接传递一个变量，这样变量值是多少就会读取那个属性
obj["123"] = 234;

删除属性
delete obj.name;

# 9. 作用域

作用域指一个变量的作用的范围,在JS中一共有两种作用域：

## 9.1. 全局作用域

直接编写在script标签中的JS代码，都在全局作用域
全局作用域在页面打开时创建，在页面关闭时销毁

 在全局作用域中有一个全局对象window，它代表的是一个浏览器的窗口，它由浏览器创建我们可以直接使用

在全局作用域中：
创建的变量都会作为window对象的属性保存
创建的函数都会作为window对象的方法保存
全局作用域中的变量都是全局变量，在页面的任意的部分都可以访问的到

## 9.2. 函数作用域

定义函数时创建函数作用域
在函数作用域中可以访问到全局作用域的变量,在全局作用域中无法访问到函数作用域的变量
当在函数作用域操作一个变量时，它会先在自身作用域中寻找，如果有就直接使用，如果没有则向上一级作用域中寻找，直到找到全局作用域，如果全局作用域中依然没有找到，则会报错ReferenceError
在函数中要访问全局变量可以使用window对象

# 10. 声明提前

## 10.1. 变量的声明提前

使用var关键字声明的变量，会在所有的代码执行之前被声明（但是不会赋值），但是如果声明变量时不适用var关键字，则变量不会被声明提前

## 10.2. 函数的声明提前

使用函数声明形式创建的函数 function 函数(){}
它会在所有的代码执行之前就被创建，所以我们可以在函数声明前来调用函数

var a = function fn() {}
使用函数表达式创建的函数，不会被声明提前，所以不能在声明前调用

# 11. this

解析器在调用函数每次都会向函数内部传递进一个隐含的参数,这个隐含的参数就是this，this指向的是一个对象

这个对象我们称为函数执行的 上下文对象，根据函数的调用环境的不同，this会指向不同的对象

一句话：this指向调用它的对象

1.以函数的形式调用时，this指向window

2.以方法的形式调用时，this就是调用方法的那个对象

3.当以构造函数的形式调用时，this就是新创建的实例对象

4.使用call() apply() bind()调用时，this自定义

5.箭头函数自身没有this，箭头函数的this继承的是定义时外层最近的对象的this 

6.严格模式下，函数this指向undefined

# 12. arguments

在调用函数时，浏览器每次都会传递进两个隐含的参数：
1.函数的上下文对象 this
2.封装实参的对象 arguments

arguments是一个 类数组 (不是数组)对象,它也可以通过索引来操作数据，也可以获取长度
在调用函数时，我们所传递的实参都会在arguments中保存
arguments.length可以用来获取实参的长度
我们即使不定义形参，也可以通过arguments来使用实参，只不过比较麻烦
arguments[0] 表示第一个实参
arguments[1] 表示第二个实参

它里边有一个属性叫做callee，这个属性对应一个函数对象，就是当前正在指向的函数的对象

```
function fun(a,b){
	console.log(arguments instanceof Array);
	console.log(Array.isArray(arguments));
	console.log(arguments[1]);
	console.log(arguments.length);
	console.log(arguments.callee == fun);
}
```
# 13. 函数 function

## 13.1. 匿名立即执行函数（IIFE）

函数定义完，立即被调用，这种函数叫做立即执行函数，立即执行函数往往只会执行一次

```
(function(a,b){
    console.log("a = "+a);
    console.log("b = "+b);
})(123,456);
```

## 13.2. 遍历对象函数

```
var obj = {
    name:"孙悟空",
    age:18,
    gender:"男",
    address:"花果山"
};

语法：
for(var n in obj){
    console.log("属性名:"+n);
    console.log("属性值:"+obj[n]);
}
```

## 13.3. 构造函数

构造函数就是一个普通的函数，创建方式和普通函数没有区别,不同的是构造函数习惯上首字母大写
构造函数和普通函数的区别就是调用方式的不同：普通函数是直接调用，而构造函数需要使用new关键字来调用

构造函数的执行流程：
1.立刻创建一个新的对象
2.将新建的对象设置为函数中this,在构造函数中可以使用this来引用新建的对象
3.逐行执行函数中的代码
4.将新建的对象作为返回值返回
使用同一个构造函数创建的对象，我们称为一类对象，也将一个构造函数称为一个类。我们将通过一个构造函数创建的对象，称为是该类的实例

```
function Person(name , age , gender) {
    this.name = name;
    this.age = age;
    this.gender = gender;
    this.sayName = function(){
        alert(this.name);
    };
}

var per1 = new Person("孙悟空",18,"男");
var per2 = new Person("玉兔精",16,"女");

使用instanceof可以检查一个对象是否是一个类的实例
语法：
    实例 instanceof 构造函数
如果是，则返回true，否则返回false

所有的对象都是Object的后代，所以任何对象和Object左instanceof检查时都会返回true
```

# 14. 原型

我们所创建的每一个函数，解析器都会向函数中添加一个属性prototype
这个属性对应着一个对象，这个对象就是我们所谓的原型对象
如果函数作为普通函数调用prototype没有任何作用
当函数以构造函数的形式调用时，它所创建的对象中都会有一个隐含的属性，指向该构造函数的原型对象，我们可以通过__proto__来访问该属性

原型对象就相当于一个公共的区域，所有同一个类的实例都可以访问到这个原型对象，我们可以将对象中共有的内容，统一设置到原型对象中。

当我们访问对象的一个属性或方法时，它会先在对象自身中寻找，如果有则直接使用，如果没有则会去原型对象中寻找，如果找到则直接使用
以后我们创建构造函数时，可以将这些对象共有的属性和方法，统一添加到构造函数的原型对象中，这样不用分别为每一个对象添加，也不会影响到全局作用域，就可以使每个对象都具有这些属性和方法了

使用 in 检查对象中是否含有某个属性时，如果对象中没有但是原型中有，也会返回true
`console.log("name" in mc);`

可以使用对象的 hasOwnProperty() 来检查对象自身中是否含有该属性，使用该方法只有当对象自身中含有属性时，才会返回true
`console.log(mc.hasOwnProperty("age"));`

原型对象也是对象，所以它也有原型，当我们使用一个对象的属性或方法时，会现在自身中寻找，
自身中如果有，则直接使用，
如果没有则去原型对象中寻找，如果原型对象中有，则使用，
如果没有则去原型的原型中寻找,直到找到Object对象的原型，
Object对象的原型没有原型，如果在Object原型中依然没有找到，则返回undefined

# 15. toString

当我们直接在页面中打印一个对象时，实际上是输出的对象的toString()方法的返回值
注意：数组的toString()方法是不一样的

如果我们希望在输出对象时不输出[object Object]，可以为对象添加一个toString()方法

# 16. 数组方法

创建数组对象：
`var arr = new Array();`
`var arr = [];`

向数组中添加元素：
```
    arr[0] = 10;
    arr[1] = 33;
    arr[2] = 22;
    arr[3] = 44;
```

读取数组中的元素，如果读取不存在的索引，他不会报错而是返回undefined

获取数组的长度：
可以使用length属性来获取数组的长度(元素的个数)
对于连续的数组，使用length可以获取到数组的长度（元素的个数）
对于非连续的数组，使用length会获取到数组的最大的索引+1

修改length
如果修改的length大于原长度，则多出部分会空出来
如果修改的length小于原长度，则多出的元素会被删除

## 16.1. push() 改变原数组

该方法可以向数组的末尾添加一个或多个元素，并返回数组的新的长度
可以将要添加的元素作为方法的参数传递，这样这些元素将会自动添加到数组的末尾
该方法会将数组新的长度作为返回值返回

## 16.2. pop() 改变原数组

该方法可以删除数组的最后一个元素,并将被删除的元素作为返回值返回

## 16.3. unshift() 改变原数组

向数组开头添加一个或多个元素，并返回新的数组长度
向前边插入元素以后，其他的元素索引会依次调整

## 16.4. shift() 改变原数组

可以删除数组的第一个元素，并将被删除的元素作为返回值返回

## 16.5. splice() 改变原数组

可以用于删除数组中的指定元素
使用splice()会影响到原数组，会将指定元素从原数组中删除,并将被删除的元素作为返回值返回

参数：
    第一个，表示开始位置的索引
    第二个，表示删除的数量
    第三个及以后可以传递一些新的元素，这些元素将会自动插入到开始位置索引前边
    
## 16.6. reverse() 改变原数组

该方法用来反转数组（前边的去后边，后边的去前边）该方法会直接修改原数组，返回该数组

## 16.7. sort() 改变原数组

可以用来对数组中的元素进行排序
也会影响原数组，默认会按照Unicode编码进行排序
即使对于纯数字的数组，使用sort()排序时，也会按照Unicode编码来排序，所以对数字进排序时，可能会得到错误的结果。
我们可以自己来指定排序的规则
我们可以在sort()添加一个回调函数，来指定排序规则，回调函数中需要定义两个形参
浏览器将会分别使用数组中的元素作为实参去调用回调函数
使用哪个元素调用不确定，但是肯定的是在数组中a一定在b前边
浏览器会根据回调函数的返回值来决定元素的顺序，
如果返回一个大于0的值，则元素会交换位置
如果返回一个小于0的值，则元素位置不变
如果返回一个0，则认为两个元素相等，也不交换位置
如果需要升序排列，则返回 a-b
如果需要降序排列，则返回 b-a

```
arr.sort(function(a,b){
    return b - a/a - b;
});
```

## 16.8. forEach() 不改变原数组

一般我们都是使用for循环去遍历数组，JS中还为我们提供了一个方法，用来遍历数组
forEach()
    这个方法只支持IE8以上的浏览器

forEach()方法需要一个函数作为参数
像这种函数，由我们创建但是不由我们调用的，我们称为回调函数
数组中有几个元素函数就会执行几次，每次执行时，浏览器会将遍历到的元素
以实参的形式传递进来，我们可以来定义形参，来读取这些内容
浏览器会在回调函数中传递三个参数：
第一个参数，就是当前正在遍历的元素
第二个参数，就是当前正在遍历的元素的索引
第三个参数，就是正在遍历的数组

```
arr.forEach(function(value , index , obj){
    console.log(value);
});
```

## 16.9. slice() 不改变原数组，浅拷贝

可以用来从数组提取指定元素
该方法不会改变元素数组，而是将截取到的元素封装到一个新数组中返回

参数：
    1.截取开始的位置的索引,包含开始索引
    2.截取结束的位置的索引,不包含结束索引,第二个参数可以省略不写,此时会截取从开始索引往后的所有元素
    索引可以传递一个负值，如果传递一个负值，则从后往前计算
    -1 倒数第一个
    -2 倒数第二个

## 16.10. concat() 不改变原数组，浅拷贝

concat()可以连接两个或多个数组，并将新的数组返回，该方法不会对原数组产生影响
`var result = arr.concat(arr2,arr3,"牛魔王","铁扇公主");`


## 16.11. join() 不改变原数组

该方法可以将数组转换为一个字符串
该方法不会对原数组产生影响，而是将转换后的字符串作为结果返回
在join()中可以指定一个字符串作为参数，这个字符串将会成为数组中元素的连接符
如果不指定连接符，则默认使用,作为连接符

```
	arr = ["孙悟空","猪八戒","沙和尚","唐僧"];
	result = arr.join("@-@");
```


## 16.12. reduce() 不改变原数组

reduce() 方法对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值。

reducer 函数接收4个参数:

Accumulator (acc) (累计器)
Current Value (cur) (当前值)
Current Index (idx) (当前索引)
Source Array (src) (源数组)

数组里所有值的和
```
var sum = [0, 1, 2, 3].reduce(function (accumulator, currentValue) {
  return accumulator + currentValue;
}, 0);
// 和为 6
```

## 16.13. every() 不改变原数组

测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值。
arr.every(callback(currentValue, index, arr)[, thisArg])

## 16.14. indexOf() 不改变原数组

返回第一个与给定参数相等的数组元素的索引，没有找到则返回-1

## 16.15. lastIndexOf() 不改变原数组

返回在数组中搜索到的与给定参数相等的元素的索引里最大的值，不改变原数组

## 16.16. map() 不改变原数组

创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。
var new_array = arr.map(function callback(currentValue[, index[, array]]) {
     Return element for new_array
}[, thisArg])

## 16.17. some() 不改变原数组

测试数组中是不是至少有1个元素通过了被提供的函数测试。它返回的是一个Boolean类型的值。
arr.some(callback(element[, index[, array]])[, thisArg])

## 16.18. filter() 不改变原数组

创建一个新数组, 它返回的新数组由使回调函数返回true的元素组成，
var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])

## 16.19. includes() 不改变原数组

用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。
arr.includes(valueToFind[, fromIndex])

## 16.20. find() 不改变原数组

方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined。
arr.find(callback[, thisArg])

## 16.21. findIndex() 不改变原数组

方法返回数组中满足提供的测试函数的第一个元素的索引。否则返回-1。
arr.findIndex(callback[, thisArg])

## 16.22. values() 不改变原数组

方法返回一个新的 Array Iterator 对象，该对象包含数组每个索引的值，不改变原数组

## 16.23. entries() 不改变原数组

方法返回一个新的Array Iterator对象，该对象包含数组中每个索引的键/值对，不改变原数组
 
## 16.24. keys() 不改变原数组

方法返回一个包含数组中每个索引键的Array Iterator对象，不改变原数组

以上三种方法返回的Iterator对象可通过.next()依次查看

# 17. Date

在JS中使用Date对象来表示一个时间
创建一个Date对象
如果直接使用构造函数创建一个Date对象，则会封装为当前代码执行的时间
`var d = new Date();`

创建一个指定的时间对象
需要在构造函数中传递一个表示时间的字符串作为参数
日期的格式  月份/日/年 时:分:秒
`var d2 = new Date("2/18/2011 11:10:30");`

## 17.1. getDate()

获取当前日期对象是几日

## 17.2. getDay()

获取当前日期对象是周几
会返回一个0-6的值
0 表示周日
1 表示周一

## 17.3. getMonth()

获取当前时间对象的月份
会返回一个0-11的值
0 表示1月
1 表示2月

## 17.4. getFullYear()

获取当前日期对象的年份

## 17.5. getTime()

获取当前日期对象的时间戳
时间戳，指的是从格林威治标准时间的1970年1月1日，0时0分0秒 到当前日期所花费的毫秒数（1秒 = 1000毫秒）
计算机底层在保存时间时使用都是时间戳

利用时间戳来测试代码的执行的性能

```
var start = Date.now();
for(var i=0 ; i<100 ; i++){
    console.log(i);
}
var end = Date.now();
console.log("执行了："+(end - start)+"毫秒");
```

# 18. Math

Math和其他的对象不同，它不是一个构造函数，它属于一个工具类不用创建对象，它里边封装了数学运算相关的属性和方法

比如
    Math.PI 表示的圆周率

## 18.1. abs()

可以用来计算一个数的绝对值

## 18.2. ceil()

可以对一个数进行向上取整，小数位只有有值就自动进1

## 18.3. floor()

可以对一个数进行向下取整，小数部分会被舍掉

## 18.4. round()

可以对一个数进行四舍五入取整

## 18.5. random()

可以用来生成一个0-1之间的随机数
生成一个0-x之间的随机数
`Math.round(Math.random()*x)`

生成一个x~y之间的随机数
`Math.round(Math.random()*(y-x)+x)`

## 18.6. max()

可以获取多个数中的最大值

## 18.7. min()

可以获取多个数中的最小值

## 18.8. pow()

`Math.pow(x,y)`	返回x的y次幂

## 18.9. sqrt()

用于对一个数进行开方运算

# 19. 包装类

在JS中为我们提供了三个包装类，通过这三个包装类可以将基本数据类型的数据转换为对象

String()
    可以将基本数据类型字符串转换为String对象

Number()
    可以将基本数据类型的数字转换为Number对象

Boolean()
    可以将基本数据类型的布尔值转换为Boolean对象

但是注意：我们在实际应用中不会使用基本数据类型的对象，如果使用基本数据类型的对象，在做一些比较时可能会带来一些不可预期的结果

方法和属性只能添加给对象，不能添加给基本数据类型
当我们对一些基本数据类型的值去调用属性和方法时，浏览器会临时使用包装类将其转换为对象，然后在调用对象的属性和方法
调用完以后，在将其转换为基本数据类型

```
var s = 123;
s = s.toString();
s.hello = "你好";
console.log(s.hello); // undefined
console.log(typeof s); // string
```

# 20. 字符串方法

`var str = "Hello Atguigu";`

在底层字符串是以字符数组的形式保存的
["H","e","l"......]
`console.log(str[4]);`

length属性
可以用来获取字符串的长度
`console.log(str.length);`

注意：字符串为基本数据类型，不可改变，所以均返回新的string

## 20.1. charAt()

可以返回字符串中指定位置的字符，根据索引获取指定的字符
`var result = str.charAt(6);`

## 20.2. charCodeAt()

获取指定位置字符的字符编码

## 20.3. formCharCode()

可以根据字符编码去获取字符

## 20.4. concat()

可以用来连接两个或多个字符串，作用和+一样

## 20.5. indexOf()

该方法可以检索一个字符串中是否含有指定内容
如果字符串中含有该内容，则会返回其第一次出现的索引
如果没有找到指定的内容，则返回-1
可以指定一个第二个参数，指定开始查找的位置

## 20.6. lastIndexOf();

该方法的用法和indexOf()一样
不同的是indexOf是从前往后找，而lastIndexOf是从后往前找
也可以指定开始查找的位置

## 20.7. slice()

可以从字符串中截取指定的内容
不会影响原字符串，而是将截取到内容返回
参数：
    第一个，开始位置的索引（包括开始位置）
    第二个，结束位置的索引（不包括结束位置）
    如果省略第二个参数，则会截取到后边所有的，也可以传递一个负数作为参数，负数的话将会从后边计算

## 20.8. substring()

可以用来截取一个字符串，可以slice()类似
参数：
    第一个：开始截取位置的索引（包括开始位置）
    第二个：结束位置的索引（不包括结束位置）
    不同的是这个方法不能接受负值作为参数，
    如果传递了一个负值，则默认使用0
    而且他还自动调整参数的位置，如果第二个参数小于第一个，则自动交换


## 20.9. split()

可以将一个字符串拆分为一个数组
参数：
    需要一个字符串作为参数，将会根据该字符串去拆分数组

```
	str = "abcbcdefghij";
	result = str.split("d");
	console.log(result);
	如果传递一个空串作为参数，则会将每个字符都拆分为数组中的一个元素
	result = str.split("");
```

## 20.10. toUpperCase()

将一个字符串转换为大写并返回

## 20.11. toLowerCase()

将一个字符串转换为小写并返回

## 20.12. trim()

从一个字符串的两端删除空白字符。在这个上下文中的空白字符是所有的空白字符

## 20.13. includes()

用于判断一个字符串是否包含在另一个字符串中，根据情况返回 true 或 false。

## 20.14. startsWith()

否以指定字符串开头，可指定搜索开始索引

## 20.15. endsWith()

是否以指定字符串结尾，可指定搜索开始索引 length - index开始

## 20.16. repeat() 

重复字符串，参数为次数

# 正则表达式

语法：
var 变量 = new RegExp("正则表达式","匹配模式");

`var reg = new RegExp("a"); 这个正则表达式可以来检查一个字符串中是否含有a`

在构造函数中可以传递一个匹配模式作为第二个参数，可以是：
    i 忽略大小写 
    g 全局匹配模式

正则表达式的方法：
test()
使用这个方法可以用来检查一个字符串是否符合正则表达式的规则，如果符合则返回true，否则返回false

使用字面量来创建正则表达式
语法：var 变量 = /正则表达式/匹配模式
使用字面量的方式创建更加简单，使用构造函数创建更加灵活

创建一个正则表达式，检查一个字符串中是否有a或b或c
`reg = /a|b|c/;`
`reg = /[abc]/;`
使用 | 表示或者的意思
[]里的内容也是或的关系
`[^ ]` 除了
`[a-z]` 任意小写字母
`[A-Z]` 任意大写字母
`[A-z]` 任意字母
`[0-9]` 任意数字
`[^0-9]` 除了数字

量词
通过量词可以设置一个内容出现的次数
量词只对它前面一个内容起作用

正好出现n次
`{n}`

a正好出现3次
`var reg = /a{3}/;`

ab正好出现3次
`var reg = /(ab){3}/;`

出现m~n次
`{m,n}`

出现m次以上
`{m,}`

`+` 表示至少一个，相当于{1,}
`var reg = /ab+c/;`

`*` 表示0个或多个，相当于{0,}

`?` 表示0个或1个，相当于{0,1}

`^` 表示开头
检查一个字符串是不是以a开头
`var reg = /^a/;`

`$` 表示结尾
检查一个字符串是不是以a结尾
`var reg = /a$/;`

`.`表示任意字符
 检查一个字符串是否含有 .
在正则表达式中使用\作为转义字符
`var reg = /\./;`

 检查一个字符串是否含有 \
` var reg = /\\/;`

注意：使用构造函数时，由于它的参数是一个字符串，而\是字符串的转义符
所以如果要使用\则需要\\来替代
`reg = new RegExp("\\");`

\w
    任意字母，数字，_

\W
    除了字母，数字，_

\d
    任意数字

\D
    除了数字

\s
    空格

\S
    除了空格

\b
    单词边界

创建一个正则表达式检查一个字符串是否含有单词child
```
reg = /\bchild\b/;
console.log(reg.test("hello children"));
```

\B
    非单词边界

去除字符串前后的空格
var reg = /^\s* | \s*$/g;


# 字符串和正则相关的方法

## split()

可以将一个字符串拆分为一个数组
方法中可以传递一个正则表达式作为参数，这样方法将会根据正则表达式去拆分字符串
这个方法即使不指定全局匹配，也会全都插分

根据任意字母来将字符串拆分
`var result = str.split(/[A-z]/);`

## search()

可以搜索字符串中是否含有指定内容
如果搜索到指定内容，则会返回第一次出现的索引，如果没有搜索到返回-1
 它可以接受一个正则表达式作为参数，然后会根据正则表达式去检索字符串
serach()只会查找第一个，即使设置全局匹配也没用

搜索字符串中是否含有 abc 或 aec 或 afc
`result = str.search(/a[bef]c/);`

## match()

可以根据正则表达式，从一个字符串中将符合条件的内容提取出来
默认情况下我们的match只会找到第一个符合要求的内容，找到以后就停止检索
我们可以设置正则表达式为全局匹配模式，这样就会匹配到所有的内容
可以为一个正则表达式设置多个匹配模式，且顺序无所谓
match()会将匹配到的内容封装到一个数组中返回，即使只查询到一个结果

```
str = "1a2a3a4a5e6f7A8B9C";
result = str.match(/[a-z]/ig);
```

## replace()

可以将字符串中指定内容替换为新的内容
参数：
1.被替换的内容，可以接受一个正则表达式作为参数
2.新的内容，默认只会替换第一个

# DOM

## 获取元素节点

通过document对象调用

### 1.getElementById() DOM节点

通过id属性获取一个元素节点对象

### 2.querySelector() DOM节点

通过CSS选择器的语法获取匹配的第一个元素节点对象

### 3.getElementsByTagName() HTMLCollection动态列表

通过标签名获取一组元素节点对象
这个方法会给我们返回一个 HTMLCollection 类数组对象

HTMLCollection 是即时更新的（live）；当其所包含的文档结构发生改变时，它会自动更新。

### 4.getElementsByClassName() HTMLCollection动态列表

不兼容IE8及以下浏览器


### 5.getElementsByName() NodeList静态列表

通过name属性获取一组元素节点对象，这个方法会给我们返回一个 NodeList 类数组对象

一般情况下是静态对象，不会自动更新

### 6.querySelectorAll NodeList静态列表

兼容IE8及以下

## 获取元素节点的子节点

通过具体的元素节点调用

### 1.childNodes

属性，表示当前节点的所有子节点（换行也算文本节点）注意：IE8及以下，不会将空白换行作为节点

### 2.firstChild

属性，表示当前节点的第一个子节点

### 3.lastChild

属性，表示当前节点的最后一个子节点

## 获取父节点和兄弟节点

通过具体的节点调用

### 1.parentNode

属性，表示当前节点的父节点

### 2.previousSibling

属性，表示当前节点的前一个兄弟节点

### 3.nextSibling

属性，表示当前节点的后一个兄弟节点

## 读取元素节点属性

直接使用 元素.属性名
注意：class属性不能采用这种方式，读取class属性时，使用元素.className

## DOM查询的剩余方法

在document中有一个属性body，它保存的是body的引用

```
var body = document.body;
console.log(body);
```

document.documentElement保存的是html根标签

document.all代表页面中所有的元素


## DOM节点增删改

### createElement()

可以用于创建一个元素节点对象
它需要一个标签名作为参数，将会根据该标签名创建元素节点对象
并将创建好的对象作为返回值返回
例如：
    创建一个li标签
`    var li1 = document.createElement("li");`

### createTextNode()

用于创建一个文本节点对象
需要一个文本内容作为参数，将会根据该内容创建文本节点并将新节点返回
例如：
    创建一个广州文本节点
`    var gzText = document.createTextNode("广州");`

### appendChild()

向一个父节点中添加一个新的子节点
用法:
    父节点.appendChild(子节点);

### insertBefore()

可以在指定的子节点前插入新的子节点
语法：
    父节点.insertBefore(新节点(前),旧节点(后));

### replaceChild()

可以使用指定子节点替换已有子节点
语法：
    父节点.replaceChild(新节点,旧节点);

### removeChild()

可以删除一个子节点
语法：
    父节点.removeChild(子节点);
    子节点.parentNode.removeChild(子节点);

### innerHTML增删改

使用innerHTML也可以完成DOM的增删改相关操作
一般会结合使用

## DOM操作元素样式

### 修改内联样式

语法：
    元素.style.样式名=样式值;
    注意：若CSS样式名中含有-，这种名称在JS中是不合法的，比如background-color
    需要将这种样式名修改为驼峰命名法，去掉-，并将-后面的首字母大写
    同时注意，我们通过style设置的样式都是内联样式，而内联样式有比较高的优先级，所以通过JS修改样式后往往会立即显示；
    如果样式中!important，此时样式有最高优先级，即使通过JS也无法改变，所以尽量不要为样式添加!important

### 读取内联样式

语法：
    元素.style.样式名;
    注意：通过style属性读取的都是内联样式

### 读取元素当前显示的样式

语法：
    元素.currentStyle.样式名
    可以用来读取当前元素正在显示的样式
    如果元素没有设置它的样式，则获取它的默认值
    注意：
    currentStyle只有IE支持，其他浏览器不支持

在其他浏览器中可以使用
    getComputedStyle()这个方法来获取元素当前的样式
    这个方法是window的方法，可以直接使用
需要两个参数
    第一个，要获取样式的元素
    第二个，可以传递一个伪元素，一般null
该方法会返回一个对象，对象中封装了当前元素对应的样式
    可以通过对象.样式名来读取样式
    如果获取的样式没有设置，则会获取到真实值而非默认值
    比如，没有设置width不会取到auto而是一个长度
    该方法不支持IE8及以下
```
var obj = getComputedStyle(box1,null);
alert(obj.width);
```

### 其他样式相关的属性

clientWidth/clientHeight
这两个属性可以获取元素的可见宽度和高度
这些属性都是不带px的，返回都是一个数字，可以直接进行计算
宽度和高度包括内容区和内边距
这些属性都是只读的，不能修改

offsetWidth/offsetHeight
获取元素的整个的宽度和高度，包括内容区，内边距和边框

offsetParent
元素本身定位为fixed：offsetParent:null（除火狐）offsetParent:body（火狐）
元素本身定位不为fixed：offsetParent为最先开启定位的祖先元素；若祖先没有定位，则offsetParent:body
body的offsetParent为null

offsetLeft/offsetTop
当前元素相对offsetParent元素的水平，垂直偏移量

scrollWidth/scrollHeight
可以获取元素整个滚动区的宽度和高度

scrollLeft/scrollTop
可以获取水平，垂直滚动条滚动的距离

如果scrollWidth - scrollLeft == clientWidth则说明滚动条滚到底了
垂直方向同理

## 事件对象

当事件的响应函数被触发时，浏览器每次都会将一个事件对象作为实参传递进响应函数
在事件对象中封装了当前事件相关的一切信息比如：鼠标的坐标，键盘哪个按键被按下，比如滚轮滚动的方向
在IE8中，响应函数被触发时，浏览器不会传递事件对象
在IE8及以下的浏览器中，是将事件对象作为window对象的属性保存的 window.event.属性/方法

解决事件对象的兼容性问题

`event = event || window.event;`

## 事件的冒泡（Bubble）

所谓的事件的冒泡指的是事件的向上传导，当后代元素上的事件被触发时，其祖先元素的相同事件也会被触发
在开发中大部分情况冒泡都是有用的，如果不希望发生事件冒泡，可以通过事件对象来取消冒泡

`event = event || window.event;`
`event.stopPropagation();`

## 事件的委派

将统一的事件绑定给元素的共同祖先元素，这样当后代元素上的事件被触发时，会一直冒泡到祖先元素，从而通过祖先元素的响应函数来处理事件

事件委派是利用了冒泡，通过委派可以减少事件绑定的次数，提高程序的性能

但是注意祖先元素不会直接处理事件，而是根据event.target得到发生事件的后代元素，通过这个后代元素调用回调函数

## 事件的绑定

DOM0
使用 对象.事件 = 函数 的形式绑定函数
它只能同时为一个元素的一个事件绑定一个响应函数
不能绑定多个，如果绑定了多个，则后面会覆盖前面的

DOM2
addEventListener()
通过这个方法也可以为元素绑定响应函数
参数：
    1.事件的字符串，不要on
    2.回调函数，当事件触发时该函数会被调用
    3.是否在捕获阶段触发事件，需要一个布尔值，一般都传false
使用这种方式可以同时为一个元素的相同事件同时绑定多个响应函数
这样当事件被触发时，响应函数将会按照函数的绑定顺序执行
addEventListener()中的this是绑定事件的对象
不支持IE8及以下

attachEvent()
在IE8中及以下可以使用attachEvent()来绑定事件
参数：
    1.事件的字符串，要on
    2.回调函数
这个方法也可以同时为一个事件绑定多个处理函数
不同的是它是后绑定先执行，执行顺序和addEventListener()相反

attachEvent()中的this是window


## 事件的传播/事件流

事件的传播分为三个阶段

1.捕获阶段
    在捕获阶段时从最外层的祖先元素，向目标元素进行事件的捕获，但是默认此时不会触发事件

2.目标阶段
    事件捕获到目标元素，捕获结束开始在目标元素上触发事件

3.冒泡阶段
    事件从目标元素向它的祖先元素传递，依次触发祖先元素上的事件

如果希望在捕获阶段就触发事件，可以将addEventListener()的第三个参数设置为true，一般情况下我们不会希望在捕获阶段触发事件，所以这个参数一般都是false

IE8及以下的浏览器中没有捕获阶段

# BOM

浏览器对象模型
BOM可以使我们通过JS来操作浏览器
在BOM中为我们提供了一组对象用来完成浏览器的操作

BOM对象
    Window
        代表的是整个浏览器的窗口，同时window也是网页中的全局对象
    Navigator
        代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器
    Location
        代表当前浏览器的地址栏信息,通过Location可以获取地址栏信息，或者操作浏览器跳转页面
    History
        代表浏览器的历史记录，可以通过该对象来操作浏览器的历史记录
        由于隐私原因，该对象不能获取到具体的历史记录，只能操作浏览器向前或向后翻页
        而且该操作只在当此访问有效
    Screen
        代表用户的屏幕的信息，通过该对象可以获取到用户的显示器的相关的信息

## Navigator

代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器,由于历史原因，Navigator对象中的大部分属性都已经不能帮助我们识别浏览器了
一般我们只会使用userAgent来判断浏览器的信息，
userAgent是一个字符串，这个字符串中包含有用来描述浏览器信息的内容，
不同的浏览器会有不同的userAgent

如果通过UserAgent不能判断，还可以通过一些浏览器中特有的对象，来判断浏览器的信息
比如：ActiveXObject

```
            var ua = navigator.userAgent;
			console.log(ua);
			if(/firefox/i.test(ua)){
				alert("你是火狐！！！");
			}else if(/chrome/i.test(ua)){
				alert("你是Chrome");
			}else if(/msie/i.test(ua)){
				alert("你是IE浏览器~~~");
			}else if("ActiveXObject" in window){
				alert("你是IE11，枪毙了你~~~");
			}
```

## History

对象可以用来操作浏览器向前或向后翻页

length属性
可以获取到当成访问的链接数量

back()方法
可以用来回退到上一个页面，作用和浏览器的回退按钮一样

forward()方法
可以跳转下一个页面，作用和浏览器的前进按钮一样

go()方法
可以用来跳转到指定的页面，它需要一个整数作为参数：
    1:表示向前跳转一个页面 相当于forward()
    2:表示向前跳转两个页面
    -1:表示向后跳转一个页面
    -2:表示向后跳转两个页面

## Location

该对象中封装了浏览器的地址栏的信息

如果直接打印location，则可以获取到地址栏的信息（当前页面的完整路径）

如果直接将location属性修改为一个完整的路径，或相对路径，则我们页面会自动跳转到该路径，并且会生成相应的历史记录
`location = "http://www.baidu.com";`

assign()方法
用来跳转到其他的页面，作用和直接修改location一样
`location.assign("http://www.baidu.com");`

reload()方法
用于重新加载当前页面，作用和刷新按钮一样
如果在方法中传递一个true，作为参数，则会强制清空缓存刷新页面

replace()
可以使用一个新的页面替换当前页面，调用完毕也会跳转页面，不会生成历史记录，不能使用回退按钮回退

# 定时器

setInterval()
定时调用
可以将一个函数，每隔一段时间被调用依次
参数：
    1.回调函数，该函数会每隔一段时间被调用一次
    2.每次调用间隔的时间，单位毫秒
`var timer = setInterval(function(){},1000);`
返回值
    返回一个Number类型的数据
    这个数字来作为定时器的唯一标识

clearInterval()
关闭定时器
`clearInterval(timer);`
可以接收任意参数，如果参数是一个有效的定时器的标识，则停止对应的定时器，如果参数不是一个有效的标识，则什么也不做

setTimeout()
延时调用
延时调用一个函数不马上执行，而是隔一段时间后再执行，而且只会只会执行一次
`var timer = setTimeout(function(){},3000);`

clearTimeout()
关闭一个延时调用

# 类的操作

修改box的class属性
我们可以通过修改元素的class属性间接修改样式
这样一来，我们只需要修改一次，即可同时修改多个样式
浏览器只需要重新渲染页面一次，性能较好
并且这种方式，可以使表现和行为进一步分离

## classList

classList是一个只读属性，返回一个元素的类属性的实时 DOMTokenList 集合
使用classList是替代className作为空格分隔的字符串访问元素的类列表的一种方便方法

### classList.add(string)

添加指定的类值，若这些类已经存在于元素的属性中，那么它们将被忽略

### classList.remove(string)

删除指定的类值

### classList.item(number)

按集合中的索引返回类值

### classList.toggle(string，force)

当只有一个参数时：切换class value;即如果类存在，则删除它并返回false,如果不存在，则添加它并返回true
当存在第二个参数时：如果第二个参数的计算结果为true;则添加指定的类值，如果计算结果为false,则删除它

### classList.contains(string)

检查元素的类属性中是否存在指定的类值

### classList.replace(oldclass, newclass)

用一个新类替换旧类

# JSON

JavaScript Object Notation JS对象表示法
JS中的对象只有JS自己认识，其他的语法都不认识
JSON就是一个特殊格式的字符串，这个字符串可以被任意语言所识别，并且可以转换为任意语言中的对象，JSON在开发中主要用于数据的交互

JSON 是一种语法，用来序列化对象、数组、数值、字符串、布尔值和 null 。它基于 JavaScript 语法，但与之不同：JavaScript不是JSON，JSON也不是JavaScript。

属性名称必须是双引号括起来的字符串；最后一个属性后不能有逗号。

常用JSON分类：
    1.对象{}
    2.数组[]

JSON允许的值：
    1.字符串
    2.数值
    3.布尔值
    4.null
    5.对象
    6.数组

```
var obj = '{
              "name" : "xjh", 
              "age" : 18, 
              "gender" : "男"
            }';

var arr = '[1, 2, 3, "hello", true]';
```

## JSON→JS对象

JSON.parse();

可以将JSON字符串转换为JS对象，需要一个JSON字符串作为参数，将字符串转为JS对象返回

```
var o1 = JSON.parse(obj);
var o2 = JSON.parse(arr);
```

## JS对象→JSON

JSON.stringfy();

可以将一个JS对象转换为JSON字符串，需要一个JS对象作为参数，将JS对象转为JSON字符串返回

`var str = JSON.stringfy(obj);`

JSON这个对象在IE7及以下不支持，若要兼容IE，可引入外部JS文件

# 小练习
## 表单全选练习

```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>全选练习</title>
<script type="text/javascript">

	window.onload = function(){
		
		
		//获取四个多选框items
		var items = document.getElementsByName("items");
		//获取全选/全不选的多选框
		var checkedAllBox = document.getElementById("checkedAllBox");
		
		/*
		 * 全选按钮
		 * 	- 点击按钮以后，四个多选框全都被选中
		 */
		
		//1.#checkedAllBtn
		//为id为checkedAllBtn的按钮绑定一个单击响应函数
		var checkedAllBtn = document.getElementById("checkedAllBtn");
		checkedAllBtn.onclick = function(){
			
			
			//遍历items
			for(var i=0 ; i<items.length ; i++){
				
				//通过多选框的checked属性可以来获取或设置多选框的选中状态
				//alert(items[i].checked);
				
				//设置四个多选框变成选中状态
				items[i].checked = true;
			}
			
			//将全选/全不选设置为选中
			checkedAllBox.checked = true;
			
			
		};
		
		/*
		 * 全不选按钮
		 * 	- 点击按钮以后，四个多选框都变成没选中的状态
		 */
		//2.#checkedNoBtn
		//为id为checkedNoBtn的按钮绑定一个单击响应函数
		var checkedNoBtn = document.getElementById("checkedNoBtn");
		checkedNoBtn.onclick = function(){
			
			for(var i=0; i<items.length ; i++){
				//将四个多选框设置为没选中的状态
				items[i].checked = false;
			}
			
			//将全选/全不选设置为不选中
			checkedAllBox.checked = false;
			
		};
		
		/*
		 * 反选按钮
		 * 	- 点击按钮以后，选中的变成没选中，没选中的变成选中
		 */
		//3.#checkedRevBtn
		var checkedRevBtn = document.getElementById("checkedRevBtn");
		checkedRevBtn.onclick = function(){
			
			//将checkedAllBox设置为选中状态
			checkedAllBox.checked = true;
			
			for(var i=0; i<items.length ; i++){
				
				//判断多选框状态
				/*if(items[i].checked){
					//证明多选框已选中，则设置为没选中状态
					items[i].checked = false;
				}else{
					//证明多选框没选中，则设置为选中状态
					items[i].checked = true;
				}*/
				
				items[i].checked = !items[i].checked;
				
				//判断四个多选框是否全选
				//只要有一个没选中则就不是全选
				if(!items[i].checked){
					//一旦进入判断，则证明不是全选状态
					//将checkedAllBox设置为没选中状态
					checkedAllBox.checked = false;
				}
			}
			
			//在反选时也需要判断四个多选框是否全都选中
			
			
			
		};
		
		/*
		 * 提交按钮：
		 * 	- 点击按钮以后，将所有选中的多选框的value属性值弹出
		 */
		//4.#sendBtn
		//为sendBtn绑定单击响应函数
		var sendBtn = document.getElementById("sendBtn");
		sendBtn.onclick = function(){
			//遍历items
			for(var i=0 ; i<items.length ; i++){
				//判断多选框是否选中
				if(items[i].checked){
					alert(items[i].value);
				}
			}
		};
		
		
		//5.#checkedAllBox
		/*
		 * 全选/全不选 多选框
		 * 	- 当它选中时，其余的也选中，当它取消时其余的也取消
		 * 
		 * 在事件的响应函数中，响应函数是给谁绑定的this就是谁
		 */
		//为checkedAllBox绑定单击响应函数
		checkedAllBox.onclick = function(){
			
			//alert(this === checkedAllBox);
			
			//设置多选框的选中状态
			for(var i=0; i <items.length ; i++){
				items[i].checked = this.checked;
			}
			
		};
		
		//6.items
		/*
		 * 如果四个多选框全都选中，则checkedAllBox也应该选中
		 * 如果四个多选框没都选中，则checkedAllBox也不应该选中
		 */
		
		//为四个多选框分别绑定点击响应函数
		for(var i=0 ; i<items.length ; i++){
			items[i].onclick = function(){
				
				//将checkedAllBox设置为选中状态
				checkedAllBox.checked = true;
				
				for(var j=0 ; j<items.length ; j++){
					//判断四个多选框是否全选
					//只要有一个没选中则就不是全选
					if(!items[j].checked){
						//一旦进入判断，则证明不是全选状态
						//将checkedAllBox设置为没选中状态
						checkedAllBox.checked = false;
						//一旦进入判断，则已经得出结果，不用再继续执行循环
						break;
					}
					
				}
				
				
				
			};
		}
		
		
	};
	
</script>
</head>
<body>

	<form method="post" action="">
		你爱好的运动是？<input type="checkbox" id="checkedAllBox" />全选/全不选 
		
		<br />
		<input type="checkbox" name="items" value="足球" />足球
		<input type="checkbox" name="items" value="篮球" />篮球
		<input type="checkbox" name="items" value="羽毛球" />羽毛球
		<input type="checkbox" name="items" value="乒乓球" />乒乓球
		<br />
		<input type="button" id="checkedAllBtn" value="全　选" />
		<input type="button" id="checkedNoBtn" value="全不选" />
		<input type="button" id="checkedRevBtn" value="反　选" />
		<input type="button" id="sendBtn" value="提　交" />
	</form>
</body>
</html>
```

## 跟随鼠标移动

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			#box1{
				width: 100px;
				height: 100px;
				background-color: red;
				/*
				 * 开启box1的绝对定位
				 */
				position: absolute;
			}
			
		</style>
		
		<script type="text/javascript">
			window.onload = function(){
				
				/*
				 * 使div可以跟随鼠标移动
				 */
				
				//获取box1
				var box1 = document.getElementById("box1");
				//绑定鼠标移动事件
				document.onmousemove = function(event){
					
					//解决兼容问题
					event = event || window.event;
					
					//获取滚动条滚动的距离
					/*
					 * chrome认为浏览器的滚动条是body的，可以通过body.scrollTop来获取
					 * 火狐等浏览器认为浏览器的滚动条是document的，
					 * 现在全是document了
					 */
					var st = document.body.scrollTop || document.documentElement.scrollTop;
					var sl = document.body.scrollLeft || document.documentElement.scrollLeft;
					//var st = document.documentElement.scrollTop;
					
					
					//获取到鼠标的坐标
					/*
					 * clientX和clientY
					 * 	用于获取鼠标在当前的可见窗口的坐标
					 * div的偏移量，是相对于整个页面的
					 * 
					 * pageX和pageY可以获取鼠标相对于当前页面的坐标
					 * 	但是这个两个属性在IE8中不支持，所以如果需要兼容IE8，则不要使用
					 */
					var left = event.clientX;
					var　top = event.clientY;
					
					//设置div的偏移量
					box1.style.left = left + sl + "px";
					box1.style.top = top + st + "px";
					
				};
				
				
			};
			
			
		</script>
	</head>
	<body style="height: 1000px;width: 2000px;">
		<div id="box1"></div>
	</body>
</html>

```

## 拖拽元素

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			#box1{
				width: 100px;
				height: 100px;
				background-color: red;
				position: absolute;
			}
			
			#box2{
				width: 100px;
				height: 100px;
				background-color: yellow;
				position: absolute;
				
				left: 200px;
				top: 200px;
			}
			
		</style>
		
		<script type="text/javascript">
			
			window.onload = function(){
				/*
				 * 拖拽box1元素
				 *  - 拖拽的流程
				 * 		1.当鼠标在被拖拽元素上按下时，开始拖拽  onmousedown
				 * 		2.当鼠标移动时被拖拽元素跟随鼠标移动 onmousemove
				 * 		3.当鼠标松开时，被拖拽元素固定在当前位置	onmouseup
				 */
				
				//获取box1
				var box1 = document.getElementById("box1");
				var box2 = document.getElementById("box2");
				var img1 = document.getElementById("img1");
				
				//开启box1的拖拽
				drag(box1);
				//开启box2的
				drag(box2);
				
				drag(img1);
				
				
				
				
			};
			
			/*
			 * 提取一个专门用来设置拖拽的函数
			 * 参数：开启拖拽的元素
			 */
			function drag(obj){
				//当鼠标在被拖拽元素上按下时，开始拖拽  onmousedown
				obj.onmousedown = function(event){
					
					//设置box1捕获所有鼠标按下的事件
					/*
					 * setCapture()
					 * 	- 只有IE支持，但是在火狐中调用时不会报错，
					 * 		而如果使用chrome调用，会报错
					 */
					/*if(box1.setCapture){
						box1.setCapture();
					}*/
					obj.setCapture && obj.setCapture();
					
					
					event = event || window.event;
					//div的偏移量 鼠标.clentX - 元素.offsetLeft
					//div的偏移量 鼠标.clentY - 元素.offsetTop
					var ol = event.clientX - obj.offsetLeft;
					var ot = event.clientY - obj.offsetTop;
					
					
					//为document绑定一个onmousemove事件
					document.onmousemove = function(event){
						event = event || window.event;
						//当鼠标移动时被拖拽元素跟随鼠标移动 onmousemove
						//获取鼠标的坐标
						var left = event.clientX - ol;
						var top = event.clientY - ot;
						
						//修改box1的位置
						obj.style.left = left+"px";
						obj.style.top = top+"px";
						
					};
					
					//为document绑定一个鼠标松开事件
					document.onmouseup = function(){
						//当鼠标松开时，被拖拽元素固定在当前位置	onmouseup
						//取消document的onmousemove事件
						document.onmousemove = null;
						//取消document的onmouseup事件
						document.onmouseup = null;
						//当鼠标松开时，取消对事件的捕获
						obj.releaseCapture && obj.releaseCapture();
					};
					
					/*
					 * 当我们拖拽一个网页中的内容时，浏览器会默认去搜索引擎中搜索内容，
					 * 	此时会导致拖拽功能的异常，这个是浏览器提供的默认行为，
					 * 	如果不希望发生这个行为，则可以通过return false来取消默认行为
					 * 
					 * 但是这招对IE8不起作用
					 */
					return false;
					
				};
			}
			
			
		</script>
	</head>
	<body>
		
		我是一段文字
		
		<div id="box1"></div>
		
		<div id="box2"></div>
		
		<img src="../img/1.jpg" id="img1" style="position: absolute;"/>
	</body>
</html>

```
## 判断元素是否滚动到底部

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			#info{
				width: 300px;
				height: 500px;
				background-color: #bfa;
				overflow: auto;
			}
			
		</style>
		<script type="text/javascript">
			window.onload = function(){
				
				/*
				 * 当垂直滚动条滚动到底时使表单项可用
				 * onscroll
				 * 	- 该事件会在元素的滚动条滚动时触发
				 */
				
				//获取id为info的p元素
				var info = document.getElementById("info");
				//获取两个表单项
				var btn01 = document.getElementById("btn01");
				var btn02 = document.getElementById("btn02");
				//为info绑定一个滚动条滚动的事件
				info.onscroll = function(){
					//检查垂直滚动条是否滚动到底
					if(info.scrollHeight - info.scrollTop === info.clientHeight){
						//滚动条滚动到底，使表单项可用
						/*
						 * disabled属性可以设置一个元素是否禁用，
						 * 	如果设置为true，则元素禁用
						 * 	如果设置为false，则元素可用
						 */
						btn01.disabled = false;
						btn02.disabled = false;
					}
					
				};
				
			};
			
			
		</script>
	</head>
	<body>
		<h3>欢迎亲爱的用户注册</h3>
		<p id="info">
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
			亲爱的用户，请仔细阅读以下协议，如果你不仔细阅读你就别注册
		</p>
		<!-- 如果为表单项添加disabled="disabled" 则表单项将变成不可用的状态 -->
		<input id="btn01" type="checkbox" disabled="disabled" />我已仔细阅读协议，一定遵守
		<input id="btn02" type="submit" value="注册" disabled="disabled" />
	</body>
</html>

```

## 滚轮事件

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			#box1{
				width: 100px;
				height: 100px;
				background-color: red;
			}
			
		</style>
		<script type="text/javascript">
			
			window.onload = function(){
				
				
				//获取id为box1的div
				var box1 = document.getElementById("box1");
				
				//为box1绑定一个鼠标滚轮滚动的事件
				/*
				 * onmousewheel鼠标滚轮滚动的事件，会在滚轮滚动时触发，
				 * 	但是火狐不支持该属性
				 * 
				 * 在火狐中需要使用 DOMMouseScroll 来绑定滚动事件
				 * 	注意该事件需要通过addEventListener()函数来绑定
				 */
				
				
				box1.onmousewheel = function(event){
					
					event = event || window.event;
					
					
					//event.wheelDelta 可以获取鼠标滚轮滚动的方向
					//向上滚 120   向下滚 -120
					//wheelDelta这个值我们不看大小，只看正负
					
					//alert(event.wheelDelta);
					
					//wheelDelta这个属性火狐中不支持
					//在火狐中使用event.detail来获取滚动的方向
					//向上滚 -3  向下滚 3
					//alert(event.detail);
					
					
					/*
					 * 当鼠标滚轮向下滚动时，box1变长
					 * 	当滚轮向上滚动时，box1变短
					 */
					//判断鼠标滚轮滚动的方向
					if(event.wheelDelta > 0 || event.detail < 0){
						//向上滚，box1变短
						box1.style.height = box1.clientHeight - 10 + "px";
						
					}else{
						//向下滚，box1变长
						box1.style.height = box1.clientHeight + 10 + "px";
					}
					
					/*
					 * 使用addEventListener()方法绑定响应函数，取消默认行为时不能使用return false
					 * 需要使用event来取消默认行为event.preventDefault();查询event.cancelable看是否可取消
					 * 但是IE8不支持event.preventDefault();如果直接调用会报错
					 */
					event.preventDefault && event.preventDefault();
					
					
					/*
					 * 当滚轮滚动时，如果浏览器有滚动条，滚动条会随之滚动，
					 * 这是浏览器的默认行为，如果不希望发生，则可以取消默认行为
					 */
					return false;
					
					
					
					
				};
				
				//为火狐绑定滚轮事件
				bind(box1,"DOMMouseScroll",box1.onmousewheel);
				
				
			};
			
			
			function bind(obj , eventStr , callback){
				if(obj.addEventListener){
					//大部分浏览器兼容的方式
					obj.addEventListener(eventStr , callback , false);
				}else{
					/*
					 * this是谁由调用方式决定
					 * callback.call(obj)
					 */
					//IE8及以下
					obj.attachEvent("on"+eventStr , function(){
						//在匿名函数中调用回调函数
						callback.call(obj);
					});
				}
			}
			
		</script>
	</head>
	<body style="height: 2000px;">
		
		<div id="box1"></div>
		
	</body>
</html>
```

## 轮播图

```
//尝试创建一个可以执行简单动画的函数
/*
 * 参数：
 * 	obj:要执行动画的对象
 * 	attr:要执行动画的样式，比如：left top width height
 * 	target:执行动画的目标位置
 * 	speed:移动的速度(正数向右移动，负数向左移动)
 *  callback:回调函数，这个函数将会在动画执行完毕以后执行
 */
function move(obj, attr, target, speed, callback) {
	//关闭上一个定时器
	clearInterval(obj.timer);

	//获取元素目前的位置
	var current = parseInt(getStyle(obj, attr));

	//判断速度的正负值
	//如果从0 向 800移动，则speed为正
	//如果从800向0移动，则speed为负
	if(current > target) {
		//此时速度应为负值
		speed = -speed;
	}

	//开启一个定时器，用来执行动画效果
	//向执行动画的对象中添加一个timer属性，用来保存它自己的定时器的标识
	obj.timer = setInterval(function() {

		//获取box1的原来的left值
		var oldValue = parseInt(getStyle(obj, attr));

		//在旧值的基础上增加
		var newValue = oldValue + speed;

		//判断newValue是否大于800
		//从800 向 0移动
		//向左移动时，需要判断newValue是否小于target
		//向右移动时，需要判断newValue是否大于target
		if((speed < 0 && newValue < target) || (speed > 0 && newValue > target)) {
			newValue = target;
		}

		//将新值设置给box1
		obj.style[attr] = newValue + "px";

		//当元素移动到0px时，使其停止执行动画
		if(newValue == target) {
			//达到目标，关闭定时器
			clearInterval(obj.timer);
			//动画执行完毕，调用回调函数
			callback && callback();
		}

	}, 30);
}

/*
 * 定义一个函数，用来获取指定元素的当前的样式
 * 参数：
 * 		obj 要获取样式的元素
 * 		name 要获取的样式名
 */
function getStyle(obj, name) {

	if(window.getComputedStyle) {
		//正常浏览器的方式，具有getComputedStyle()方法
		return getComputedStyle(obj, null)[name];
	} else {
		//IE8的方式，没有getComputedStyle()方法
		return obj.currentStyle[name];
	}

}

//定义一个函数，用来向一个元素中添加指定的class属性值
/*
 * 参数:
 * 	obj 要添加class属性的元素
 *  cn 要添加的class值
 * 	
 */
function addClass(obj, cn) {

	//检查obj中是否含有cn
	if(!hasClass(obj, cn)) {
		obj.className += " " + cn;
	}

}

/*
 * 判断一个元素中是否含有指定的class属性值
 * 	如果有该class，则返回true，没有则返回false
 * 	
 */
function hasClass(obj, cn) {

	//判断obj中有没有cn class
	//创建一个正则表达式
	//var reg = /\bb2\b/;
	var reg = new RegExp("\\b" + cn + "\\b");

	return reg.test(obj.className);

}

/*
 * 删除一个元素中的指定的class属性
 */
function removeClass(obj, cn) {
	//创建一个正则表达式
	var reg = new RegExp("\\b" + cn + "\\b");

	//删除class
	obj.className = obj.className.replace(reg, "");

}

/*
 * toggleClass可以用来切换一个类
 * 	如果元素中具有该类，则删除
 * 	如果元素中没有该类，则添加
 */
function toggleClass(obj, cn) {

	//判断obj中是否含有cn
	if(hasClass(obj, cn)) {
		//有，则删除
		removeClass(obj, cn);
	} else {
		//没有，则添加
		addClass(obj, cn);
	}

}

<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title></title>

    <style type="text/css">
        *{
            margin: 0;
            padding: 0;
        }

        /*
         * 设置outer的样式
         */
        #outer{
            /*设置宽和高*/
            width: 520px;
            height: 333px;
            /*居中*/
            margin: 50px auto;
            /*设置背景颜色*/
            background-color: greenyellow;
            /*设置padding*/
            padding: 10px 0;
            /*开启相对定位*/
            position: relative;
            /*裁剪溢出的内容*/
            overflow: hidden;
        }

        /*设置imgList*/
        #imgList{
            /*去除项目符号*/
            list-style: none;
            /*设置ul的宽度*/
            /*width: 2600px;*/
            /*开启绝对定位*/
            position: absolute;
            /*设置偏移量*/
            /*
             * 每向左移动520px，就会显示到下一张图片
             */
            left: 0px;
        }

        /*设置图片中的li*/
        #imgList li{
            /*设置浮动*/
            float: left;
            /*设置左右外边距*/
            margin: 0 10px;
        }

        /*设置导航按钮*/
        #navDiv{
            /*开启绝对定位*/
            position: absolute;
            /*设置位置*/
            bottom: 15px;
            /*设置left值
                 outer宽度  520
                 navDiv宽度 25*5 = 125
                     520 - 125 = 395/2 = 197.5
             * */
            /*left: 197px;*/
        }

        #navDiv a{
            /*设置超链接浮动*/
            float: left;
            /*设置超链接的宽和高*/
            width: 15px;
            height: 15px;
            /*设置背景颜色*/
            background-color: red;
            /*设置左右外边距*/
            margin: 0 5px;
            /*设置透明*/
            opacity: 0.5;
            /*兼容IE8透明*/
            filter: alpha(opacity=50);
        }

        /*设置鼠标移入的效果*/
        #navDiv a:hover{
            background-color: black;
        }
    </style>

    <!--引用工具-->
    <script type="text/javascript" src="js/tools.js"></script>
    <script type="text/javascript">
        window.onload = function(){
            //获取imgList
            var imgList = document.getElementById("imgList");
            //获取页面中所有的img标签
            var imgArr = document.getElementsByTagName("img");
            //设置imgList的宽度
            imgList.style.width = 520*imgArr.length+"px";


            /*设置导航按钮居中*/
            //获取navDiv
            var navDiv = document.getElementById("navDiv");
            //获取outer
            var outer = document.getElementById("outer");
            //设置navDiv的left值
            navDiv.style.left = (outer.offsetWidth - navDiv.offsetWidth)/2 + "px";

            //默认显示图片的索引
            var index = 0;
            //获取所有的a
            var allA = document.getElementsByTagName("a");
            //设置默认选中的效果
            allA[index].style.backgroundColor = "black";

            /*
                 点击超链接切换到指定的图片
                     点击第一个超链接，显示第一个图片
                     点击第二个超链接，显示第二个图片
             * */

            //为所有的超链接都绑定单击响应函数
            for(var i=0; i<allA.length ; i++){

                //为每一个超链接都添加一个num属性
                allA[i].num = i;

                //为超链接绑定单击响应函数
                allA[i].onclick = function(){

                    //关闭自动切换的定时器
                    clearInterval(timer);
                    //获取点击超链接的索引,并将其设置为index
                    index = this.num;

                    //切换图片
                    /*
                     * 第一张  0 0
                     * 第二张  1 -520
                     * 第三张  2 -1040
                     */
                    //imgList.style.left = -520*index + "px";
                    //设置选中的a
                    setA();

                    //使用move函数来切换图片
                    move(imgList , "left" , -520*index , 20 , function(){
                        //动画执行完毕，开启自动切换
                        autoChange();
                    });

                };
            }


            //开启自动切换图片
            autoChange();


            //创建一个方法用来设置选中的a
            function setA(){

                //判断当前索引是否是最后一张图片
                if(index >= imgArr.length - 1){
                    //则将index设置为0
                    index = 0;

                    //此时显示的最后一张图片，而最后一张图片和第一张是一摸一样
                    //通过CSS将最后一张切换成第一张
                    imgList.style.left = 0;
                }

                //遍历所有a，并将它们的背景颜色设置为红色
                for(var i=0 ; i<allA.length ; i++){
                    allA[i].style.backgroundColor = "";
                }

                //将选中的a设置为黑色
                allA[index].style.backgroundColor = "black";
            };

            //定义一个自动切换的定时器的标识
            var timer;
            //创建一个函数，用来开启自动切换图片
            function autoChange(){

                //开启一个定时器，用来定时去切换图片
                timer = setInterval(function(){

                    //使索引自增
                    index++;

                    //判断index的值
                    index %= imgArr.length;

                    //执行动画，切换图片
                    move(imgList , "left" , -520*index , 20 , function(){
                        //修改导航按钮
                        setA();
                    });

                },3000);

            }


        };

    </script>
</head>
<body>
<!-- 创建一个外部的div，来作为大的容器 -->
<div id="outer">
    <!-- 创建一个ul，用于放置图片 -->
    <ul id="imgList">
        <li><img src="img/1.jpg"/></li>
        <li><img src="img/2.jpg"/></li>
        <li><img src="img/3.jpg"/></li>
        <li><img src="img/4.jpg"/></li>
        <li><img src="img/5.jpg"/></li>
        <li><img src="img/1.jpg"/></li>
    </ul>
    <!--创建导航按钮-->
    <div id="navDiv">
        <a href="javascript:;"></a>
        <a href="javascript:;"></a>
        <a href="javascript:;"></a>
        <a href="javascript:;"></a>
        <a href="javascript:;"></a>
    </div>
</div>
</body>
</html>

```
