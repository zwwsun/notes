# notice

## "use strict"

- 不允许直接对未声明的变量赋值（对象也是个变量）
- 不允许删除变量和函数
- 不允许变量重名
- 不允许使用八进制和转义字符

## float类型数据实际是无限趋近的，不准确。

```javascript
    var x = 0.1;
    var y = 0.2;
    var z = x + y            // z 的结果为 0.3
    if (z == 0.3)            // 返回 false

    //可以用整数的乘除法来解决
    var z = (x * 10 + y * 10) / 10;       // z 的结果为 0.3
```

## 对象为引用类型

```javascript
    var c = [];
    var a = {};
    b.foreach( (item) => {
        a.key1 = item.key1;
        c.push(a); //错误，不能在遍历里对遍历外的对象赋值，引用类型会被不断覆盖，至始至终只有{key1: item.key1}
    })
```

## 在 JavaScript中, 只有对象使用名字作为索引。数组如果用名字做为index,会被重新定义为对象，不能再使用数组的方法和属性

## 对象只有被定义才有可能为 null，否则为 undefined。

- if (typeof myObj !== "undefined" && myObj !== null)  我们需要先使用 typeof 来检测对象是否已定义

## 函数显式参数(Parameters)与隐式参数(Arguments)

- 函数显式参数在函数定义时列出，函数隐式参数在函数调用时传递给函数真正的值。
- 如果函数在调用时未提供隐式参数，参数会默认设置为： undefined

```jaicascript
    function myFunction(x, y) {
        y = y || 0;
    }
    (x, y)里的y为Parameters, 内部的y = y || 0 都是 Arguments,为调用时传递给函数真正的值
```

## 值传递

- 在函数中调用的参数是函数的隐式参数
- JavaScript 隐式参数通过值来传递：函数仅仅只是获取值
- 如果函数修改参数的值，不会修改显式参数的初始值（在函数外定义）
- 隐式参数的改变在函数外是不可见的

## 对象传递

- 在函数内部修改对象参数的属性就会修改其初始的值
- 修改对象属性可作用于函数外部（全局变量）
- 修改对象属性在函数外是可见的

## 鼠标点击获取坐标

### event.screenX/Y

- 获取到的是触发点相对显示器屏幕左上角的距离，不随页面滚动而改变

### event.clientX/Y

- 获取到的是触发点相对浏览器可视区域左上角距离，不随页面滚动而改变

### event.pageX/Y

- 获取到的是触发点相对文档区域左上角距离，会随着页面滚动而改变。
- IE6/7/8不支持

### event.offsetX/Y

- 获取到是触发点相对被触发dom的左上角距离，不过左上角基准点在不同浏览器中有区别，其中在IE中以内容区左上角为基准点不包括边框，如果触发点在边框上会返回负值，而chrome中以边框左上角为基准点。
- Firefox不支持

### event.layerX/Y

- 获取到的是触发点相对被触发dom左上角的距离，数值与offsetX/Y相同，基准点为边框左上角，被触发的dom需要设置为position:relative或者position:absolute，否则会返回相对html文档区域左上角的距离
- IE6/7/8  opera不支持

## 宽度

### 不包括外边距

- div.offsetWidth
- $().outerWidth(false) （参数为true，包括外边距）

### 不包括边框和外边距

- div.clientWidth
- $().innerWidth()

### 不包括边框和外边距,但包括溢出尺寸（滚动时候的不可见部分）

- div.scrollWidth   无溢出的情况，与clientWidth相同

### 不包括内边距、边框和外边距

- div.style.width
- $().width()

### screen.width/height

- 屏幕分辨率的宽高

### screen.availWidth/availHeight

- 屏幕分辨率的可用宽高

### 注意

- div.style.width必须要事先定义，且定义在html里，否则取到的值为空，返回的是字符串带'px'
- div.style.width  div.scrollWidth 可读写属性
- div.clientWidth  div.offsetWidth 只读属性

## 位置

### div.offsetLeft/div.offsetTop

- 获取离其最近的已经定位的父元素或祖先元素定位，否则就是body

### div.scrollLeft/div.scrollTop

- 指向自己被卷去的宽度/高度

### $().position().left/$().position().top

- 返回的是相对于其定位的祖辈元素的坐标 包括滚动条卷去的高度

### $().offset().left/$().offset().top

- 返回或设置匹配元素相对于文档的坐标 包括滚动条卷去的高度

### div.getBoundingClientRect() 可以和event.clientX结合用

- 获取某个元素相对于视窗的位置集合。集合中有top, right, bottom, left等属性
- IE5以上兼容
- 解决position:sticky兼容性问题

### window.screenLeft/window.screenTop

- 返回窗口相对于屏幕的X和Y坐标。

## 浏览器语言

- navigator 对象包含有关浏览器的信息，以下为对象中的这几个与 language 相关的属性
  - language：返回当前的浏览器语言  （IE6-8不支持）
  - userLanguage：返回操作系统设定的自然语言    （Firefox不支持）
  - browserLanguage：返回当前的浏览器语言       （Firefox不支持）
  - systemLanguage：返回当前操作系统的缺省语言  （Firefox不支持）（Opera不支持）
- currentLang = (navigator.language || navigator.browserLanguage).toLowerCase()

## new Date()

- 国际标准化组织的国际标准ISO8601是日期和时间的表示方法
- 参数加入时区限制，则不论在世界何地调用newDate()方法，参数所代表的的都是该时区的时间，再自动转换成当地时间
- 参数不足两位需要补0
- 先用 new Date('2018-10-10T11:11:11+0800') 打印出完整的时间表示字符 'Wed Oct 10 2018 11:11:11 GMT+0800', 在以 new Date('Wed Oct 10 2018 11:11:11 GMT+0800')运行，才不存在兼容性问题
- 注意 new Date('2018-10-10T11:11:11+0800') 东八区北京时间 输出 Wed Oct 10 2018 11:11:11 GMT+0800 (中国标准时间) 这种方法在ios系统下会因为‘-’号识别失败而不生效，只能使用‘/’

## 换行符

- window系统下默认换行符为 \r\n   mac系统下默认换行符为 \r    linux系统下默认换行符为 \n
- vscode右下角 CRLF代表 \r\n  LF代表\n  可以在用户设置里搜索eol，设置全局默认的换行符
- dos2unix-7.4.0-win64.zip  工具包
  - find '.' -type f -name '*.js' -exec dos2unix.exe -D utf8 {} \; 能把当前目录及子目录下所有的js文件换行都替换成LF   dos2unix.exe搜索的是env，没设置就写相对路径
- git 设置
  - 提交时转换为LF，检出时不转换
    - git config --global core.autocrlf input
  - 拒绝提交包含混合换行符的文件
    - git config --global core.safecrlf true

## css3 transform 问题

- 进行2D或者3D动画使用transform实际上是进行矩阵运算，对应的属性为transform: matrix(...)
- transform: translate() 方法中如果使用了百分比，则如果目标元素高宽为奇数，容易出现先matrix值为小数，即导致内容文字模糊

## 子元素需要监听父元素尺寸变化

- 可以使用webAPI **MutationObserver** 构造函数进行配置，但是只能监听写在内联的style属性里的值变化
- **npm install element-resize-detector** 支持监听父元素 css样式中的变化