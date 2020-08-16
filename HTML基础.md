# `<!DOCTYPE html>`

H5的文档声明，声明当前网页是按照HTML5标准编写的
H5不再是SGML的子集，所以不再需要对DTD进行引用
编写网页时一定要将H5的文档声明写在网页最上边
如果不写文档声明，有些浏览器则会进入怪异模式/兼容模式
标准模式的网页排版和JS运作模式都是以该浏览器支持的最高标准运行。
在兼容模式中，页面以宽松的向后兼容的方式显示，模拟老式浏览器的行为以防止站点无法工作。
如在IE6中不写文档声明，则盒模型为IE盒模型，而非W3C盒模型

进入怪异模式以后，浏览器解析页面会导致页面无法正常显示，所以为了避免进入该模式，一定要写文档声明


# `<html></html>`

html根标签，一个页面中有且只有一个根标签,网页中的所有内容都应该写在html根标签中

## `<head></head>`

### `<meta/>`

meta标签用来设置网页的一些元数据，比如网页的字符集，关键字、简介等

设置网页字符集
`<meta charset="UTF-8" />`

设置网页关键字
`<meta name="keywords" content="网页的关键字" />`

设置网页的描述
`<meta name="description" content="网页的描述" />`

搜索引擎在检索页面时，会同时检索页面中的关键词和描述，利于SEO

设置请求重定向在数秒后转入目标路径
`<meta http-equiv="refresh" content="秒数;url=目标路径" />`


### `<title></title>`

title是网页的标题标签，默认会显示在浏览器的标题栏中
搜索引擎在检索页面时，会首先检索title标签中的内容
它是网页中对于搜索引擎来说最重要的内容，利于SEO

### `<style></style>`

内嵌样式：可以将CSS样式编写到head中的style标签里
```
<style type="text/css">
    p {
        color: red;
        font-size: 40px;
    }
</style>
```

### `<link/>`

外联样式：主要用于引入外部CSS文件
`<link rel="stylesheet" type="text/css" href="style.css" />`

## `<body></body>`

### 块元素 和 内联元素/行内元素

所谓块元素就是会独占一行的元素，无论他的内容多少
常见的块元素：div p h1~6 ul ol li
div这个标签没有任何语义，是一个纯粹的块元素，不会为它里边的元素设置任何的默认样式
div元素主要用来对页面进行布局

内联元素/行内元素，指的是只占自身大小的元素，不会占用一行
常见的内联元素：a img iframe span input i cite strong em sub sup
span没有任何语义，span标签专门用来选中文字，然后为文字设置样式

### 实体/转义字符串

在HTML中，如大于号小于号这种特殊符号是不能直接使用
需要一些特殊的符号来表示这些特殊符号，这些特殊的符号称为实体/转义字符串
实体语法：
&实体名字;（可以通过W3School离线手册 HTML5参考手册-HTML 符号查询）
例如:
`<`           `&lt;`
`>`           `&gt;`
 空格         `&nbsp;`
 版权符号  `&copy;`

### 常用标签属性

id属性：元素的id属性是唯一

class属性：元素的class属性可以重复，可以同时为一个元素设置多个class属性

title属性：当鼠标移到元素上时，元素中的title属性值将会作为提示文字显示

### 文本标签

body标签用来设置网页的主体内容，网页中所有可见的内容，都应该在body中编写

#### `<h1~6></h1~6>`

6级标题标签
在显示效果上h1最大，h6最小
使用HTML标签时，关心的是标签语义，我们使用的标签都是语义化标签
h1最重要，它表示一个网页中的主要内容，h2~h6的重要性依次降低
对于搜索引擎来说，h1的重要性仅次于title，搜索引擎检索完title，会立即查看h1中的内容
所以，h1标签非常重要，它会影响到页面在搜索引擎中的排名，利于SEO，页面只能写一个h1
一般页面中标题标签中h3以后基本不使用

#### `<p></p>`

p标签是段落标签，段落标签用于表示内容中的一个自然段
p标签中的文字默认会独占一行，并且两个p标签之间默认会有间距

#### `<em></em>`

文本标签em标签表示语气上的强调，浏览器默认使用斜体显示

#### `<strong></strong>`

文本标签strong表示强调的内容，比em更强烈，默认使用粗体显示

#### `<i></i>`

文本标签i标签中的内容会以斜体显示

#### `<b></b>`

文本标签b标签中的内容会以加粗显示

#### `<small></small>`

文本标签small标签中的内容会比它父元素的文字要小一些，比如：合同中的小字，网站的版权声明都可以放到small标签中

#### `<cite></cite>`

文本标签cite标签表示参考的内容，网页中所有的加书名号的内容都可以用cite标签，比如：书名，歌名，话剧名，电影名...

#### `<q></q>`

文本标签q标签表示一个短的引用（行内引用）
q标签引用的内容，浏览器会默认加上引号

#### `<blockquote></blockquote>`

文本标签blockquote标签表示一个长引用（块级引用）

#### `<sup></sup>`

使用文本标签sup标签来设置一个上标

#### `<sub></sub>`

使用文本标签sub标签用来表示一个下标

#### `<del></del>`

文本标签del标签表示一个删除的内容，del标签中的内容会自动添加删除线

#### `<ins></ins>`

文本标签ins表示一个插入的内容，ins标签中的内容会自动添加下划线

#### `<pre></pre>`

pre是一个预格式标签，会将代码中的格式保存，不会忽略多个空格

#### `<code></code>`

code标签专门用来表示代码
一般结合使用pre和code标签来表示一段代码

### 列表标签

#### 无序列表

```
<ul>
    <li> </li>
    <li> </li>
    <li> </li>
    <li> </li>
</ul>
```
通过ul标签的type属性可以修改无序列表的项目符号
可选值：
disc，默认值，实心的圆点
square，实心的方块
circle，空心的圆

注意：默认项目符号我们一般不使用，如果需要设置项目符号，可以采用为li设置背景图片的方式来设置，ul和li都是块元素

去掉项目符号
```
ul {
    list-style: none;
}
```

#### 有序列表

```
<ol type="1">
    <li> </li>
    <li> </li>
    <li> </li>
    <li> </li>
</ol>
```
有序列表使用有序的序号作为项目符号
type属性，可以指定序号的类型
可选值：
1，默认值，使用阿拉伯数字
a/A 采用小写或大写字母作为序号
i/I 采用小写或大写的罗马数字作为序号
ol也是块元素

#### 定义列表

```
<dl>
    <dt>武松</dt>
    <dd>景阳冈打虎英雄</dd>
    <dt>武大</dt>
    <dd>著名餐饮企业家</dd>
</dl>
```
定义列表用来对一些词汇或内容进行定义
使用dl来创建一个定义列表
dl中有两个子标签
dt ： 被定义的内容
dd ： 对定义内容的描述
dl,dt,dd都是块元素

### 表格标签

```
<table>
    <!-- 使用table标签中
         tr表示表格中的一行
           -td表示表格中的一列
           -th表示表头中的内容
    -->
    <!-- 有一些情况下表格是非常的长的
         这时就需要将表格分为三个部分：表头，表格主体，表格底部
         thead 表头
         tbody 表格主体
         tfoot 表格底部
         它们都是table的子标签，都需要直接写到table中，再写tr,td
         如果表格中没有写tbody，浏览器会自动在表格中添加tbody
         并且将所有的tr都放到tbody中，所以注意tr不是table的子元素，而是tbody的子元素
         通过table > tr无法选中
         需要通过tbody > tr
    -->
    <thead>
        <tr>
            <th>第1列</th>
            <th>第2列</th>
            <th>第3列</th>
            <th>第4列</th>
        </tr>
    </thead>
    
    <tfoot>
        <tr>
            <td>我是表格底部</td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tfoot>
    
    <tr>
        <td>1行1列</td>
        <td>1行2列</td>
        <td>1行3列</td>
        <td>1行4列</td>
    </tr>
    <tr>
        <td>2行1列</td>
        <td>2行2列</td>
        <td>2行3列</td>
        <!-- rowspan设置纵向的合并单元格(行合并) -->
        <td rowspan="2">2/3行4列</td>
    </tr>
    <tr>
        <td>3行1列</td>
        <td>3行2列</td>
        <td>3行3列</td>
    </tr>
    <tr>
        <td>4行1列</td>
        <td>4行2列</td>
        <!-- colspan设置横向的合并单元格(列合并) -->
        <td colspan="2">4行3/4列</td>
    </tr>
</table>
```

### 表单标签

```
    <!-- 表单的作用就是用来将用户信息提交给服务器 -->
    <!-- 使用form标签创建一个表单
         form标签中指定一个action属性
         该属性指向的是一个服务器的地址
         当我们提交表单时将会提交到action属性对应的地址
    -->
    <form action="服务器地址">
        <!--
            使用form创建的仅仅是一个空白的表单
            我们还需向form中添加不同的表单项
            在表单中可以使用fieldset来为表单项进行分组
            可以将表单项中的同一组放到一个fieldset中

            Form提供了两种数据传输的方式get和post，Get是Form的默认方法。
            
        -->
        <fieldset>
            <!-- 在fieldset中使用legend子标签来指定组名 -->
            <legend>用户信息</legend>

            <!--
            使用input来创建一个文本框，他的type属性是text
            如果希望表单项中的数据会提交到服务器中，还必须给表单项指定一个name属性
            name表示提交内容的名字
            用户填写的信息会附在url地址的后边以查询字符串的形式发送给服务器
            url地址?查询字符串
            格式：
                属性名=属性值&属性名=属性值&属性名=属性值&属性名=属性值
            在文本框中也可以指定value属性值，该值将会作为文本框的默认值显示
            -->

            <!--
            label标签可专门用来选中表单中的提示文字的
            该标签可以指定一个for属性，该属性的值需要指定一个表单项的id值
            -->
            <label for="um">用户名</label>
            <input id="um" type="text" name="username" />

            <!--
            密码框
                使用input创建一个密码框，它的type属性值是password
            -->
            <label for="pwd">密码</label>
            <input id="pwd" type="password" name="password" />
        </fieldseet>

        <fieldset>
            <legend>用户爱好</legend>

            <!--
                单选按钮
                    使用input创建单选按钮，它的type属性使用radio
                    单选按钮通过name属性进行分组，name属性相同是一组按钮
                    像这样需要用户选择但是不需要用户直接填写内容的表单项，还必须指定一个value属性，
                    这样被选中的表单项的value属性值将会最终提交给服务器

                如果希望在单选按钮或者是多选框中指定默认选中的选项
                    则可以在希望选中的项中添加checked="checked"属性
            -->
            性别 <input type="radio" name="gender" value="male" id="male" /><label for="male">男</label>
                <input type="radio" name="gender" value="female" checked="checked" id="female" /><label for="female">女</label>

            <!--
                多选框
                    使用input创建一个多选框，它的type属性使用checkbox
            -->
            爱好<input type="checkbox" name="hobby" value="zuqiu" />足球
                <input type="checkbox" name="hobby" value="lanqiu" />篮球
                <input type="checkbox" name="hobby" value="yumaoqiu" checked="checked" />羽毛球
                <input type="checkbox" name="hobby" value="ppq" checked="checked"/>乒乓球
        </fieldset>

        <!--
            下拉列表
                使用select来创建一个下拉列表
                下拉列表的name属性需要给select，而value属性需要指定给option
                可以通过在option中添加selected="selected"来将选项设置为默认选中
                当为select添加一个multiple="multiple",则下拉列表变为一个多选的下拉列表
        -->
        你喜欢的明星
            <select name="star">

            <!--
                在select中可以使用optgroup对选项进行分组
                同一个optgroup内的选项是一组
                可以通过label属性来指定分组的名字
            -->
            <optgroup label="女明星">
                <!-- 在下拉列表中使用option标签来创建一个一个列表项 -->
                <option value="fbb">范冰冰</option>
                <option value="lxr">林心如</option>
                <option value="zw">赵薇</option>
            </optgroup>

            <optgroup label="男明星">
                <option value="zbs" selected="selected">赵本山</option>
                <option value="ldh">刘德华</option>
                <option value="pcj">潘长江</option>
            </optgroup>

            </select>

        <!--使用textarea创建一个文本域-->
        自我介绍<textarea name="info"></textarea>

        <!--
            提交按钮可以将表单中的信息提交给服务器
            使用input创建一个提交按钮，它的type属性是submit
            在提交按钮中可以通过value属性来指定按钮上的文字
        -->
        <input type="submit" value="注册" />

        <!--
            <input type="reset" />可以创建一个重置按钮，
			点击重置按钮以后表单中内容将会恢复为默认值
        -->
        <input type="reset" />

        <!--
            使用input type=button可以用来创建一个单纯的按钮，
			这个按钮没有任何功能，只能被点击
        -->
        <input type="button" value="这是按钮" />

        <!--
            除了使用input，还可以使用button标签来创建按钮
            这种方式和使用input类似，只不过由于它是成对出现的标签，使用起来更加的灵活
        -->
        <button type="submit">提交</button>
        <button type="reset">重置</button>
        <button type="button">按钮</button>

    </form>
```

### `<br/>`

在HTML中，字符之间写再多的空格，浏览器也会当成一个空格解析，换行也会当成一个空格解析
所以在页面中可以使用br标签来表示换行

### `<hr/>`

hr标签可以在页面中生成一条水平线

### `<img/>`

`<img src="这是一张GIF.gif" alt="这是一张GIF图片" />`
使用img标签来向网页中引入一个外部图片
img属于行内替换元素。height/width/padding/margin均可用。效果等于块元素。
属性：
src：设置一个外部图片的路径，可以使用相对路径
alt：可以用来设置在图片不能显示时，对图片的描述，搜索引擎可以通过alt属性来识别不同的图片
如果不写alt的属性，则搜索引擎不会对img中的图片进行收录
width与height通过CSS进行设置

图片的格式：
JPEG(JPG)：
JPG图片支持的颜色比较多，图片可以压缩，但不支持透明
一般使用JPG来保存照片等颜色丰富的图片
GIF：
GIF支持的颜色少，只支持简单的透明，支持动态图
图片颜色单一或者是动态图时可以使用GIF
PNG：
PNG支持的颜色多，并且支持复杂的透明
可以用来显示颜色复杂的透明图片

图片的使用原则：
效果不一致，选择效果好的
效果一致，选择质量小的

### `<a></a>`

`<a href="目标地址" target="_blank">我是一个超链接</a>`
使用超链接可以让我们从一个页面跳转到另一个页面
属性：
    href：指向链接跳转的目标地址，可以写一个相对路径也可以是完整地址
    target：
        可选值：
        _self：在当前窗口打开（默认值）
        _blank：在新的窗口打开链接

可以设置一个内联框架的name属性，链接将会在指定的内联框架中打开

### `<iframe></iframe>`

`<iframe src="index.html" name="index"></iframe>`
使用内联框架可以引入一个外部页面
属性:
src：指向一个外部页面的路径，可以使用相对路径
name：可以为内联框架指定一个name属性
width与height通过CSS设置
不推荐使用，因为内联框架中的内容不会被搜索引擎检索
