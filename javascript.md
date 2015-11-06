# javascript笔记
## 面向对象

### 属性检测
+ in运算符:对对象的自有属性或继承属性返回true
+ hasOwnpreperty()：对自有属性返回true
+ propertyIsEnumerable()：对可枚举的自有属性返回true
+ 通过for/in遍历出所有的可枚举属性
+ ES5中Object.keys()返回一个数组，返回对象自有可枚举属性名称;Object.getOwnpropertyNames()返回所有自有属性名称

### 序列化对象
ES5内置函数：JSON.stringify()/JSON.parse()

## 数组

### 数组方法
+ join()：把数组种所有元素转化为字符串并连接在一起，返回最后生成的字符串。可以指定一个可选的字符串来分隔的元素。
+ reverse()：将数组的元素颠倒顺序
+ sort():将数组中的元素排序，默认按字母表排序，可以给sort()传递一个比较函数，按指定方式排序。
+ concat()：创建并返回一个数组，包含原数组元素和concat()的每个参数
+ slice():返回数组的片段
+ splice():在数组插入或删除元素的方法
+ push()/pop():在数组尾部添加元素返回新的长度/删除数组最后一个元素减小长度返回删除的值
+ unshift()/shift()：在头部添加/删除元素
+ toString()/toLocalString():将每个元素转化为字符串输出

#### ES5中的数组方法
+ forEach():遍历数组，为每个元素调用指定的函数
+ map():为每个元素传递给指定的函数，并返回一个数组，它包含该函数的返回值
+ filter():返回调用数组的子集，传递的函数是逻辑判定的函数，返回true/false
+ every()/some():数组种所有/存在元素满足条件
+ reduce()/reduceRight():
+ indexOf()/lastIndexOf():从头/从尾开始搜索给定值的元素

### 数组类型检测
+ ES5中isArray();
+ ES3中:
```
var isAraay = function.isArray || function(o){
	return typeof o === "object" &&
    Object.protype.toString.call(o) === "[object Array]";
}
```

## BOM:浏览器对象模型
### window对象（BOM的核心对象）
在浏览器中，即是js访问浏览器窗口的接口，又是ES规定的Global对象，有权访问parseInt()等方法
#### 全局作用域
全局作用域中声明的变量、函数都会成为window对象的属性和方法。但是全局变量不能通过delete删除，直接在window对象上定义的可以，如：
```
var age = 29;
window.color = red;
delete(window.age);//ie<9抛出错误，其他浏览器返回false
delete(window.color);//ie<9抛出错误，其他返回true
```
使用var语句添加的window属性会把[[Configurable]]设置为false，所以不能使用delete删除。

#### 窗口关系及框架
如果页面中包含框架，每个框架都有自己的window对象，保存在frames集合中，可以通过数值索引或框架名称来访问相应window对象，每个window对象都有一个name属性，其中包含框架名称。top对象指向最外层的框架。与top相对的另一个window对象parent，指向其上层框架。除非最高层窗口实通过window.open()打开的，否在其window的那么name属性不会包含任何值。
另一个与框架有关的对象self，指向window

#### 窗口位置
确定和修改window对象的属性与方法很多。
+ ie、safari、opera、chorme提供了screenLeft、screenTop属性，表示窗口相对屏幕左/上的位置。
+ firefox提供了screenX/screenY来表示
取得窗口左边和上边的位置
```
var leftPos = (type of window.screenLeft == "number") ? window.screenLeft : window.screenX;
var topPos = (type of window.screenTop == "number") ? window.screenTop : window.screenY;
```
moveTo()\moveBy():把窗口移动到/多少，接受两个参数，这两个方法可能被禁用opera，ie7及以上默认禁用。

#### 窗口大小
+ ie9+、firfox、safari、opera、chrome提供四个属性：innerWidth、innerHeight、outerWidth、outerHeight。
+ ie9+、firfox、safari中outerWidth、outerHeight总是返回浏览器窗口大小，opera中由标签页对应的浏览器窗口大小;innerWidth、innerHeight则返回页面试图区大小（减去边框）。
+ chrome中innerWidth、innerHeight、outerWidth、outerHeight返回相同值，即视口（viewport）大小。
+ ie、firfox、safari、opera、chrome中documet.documentElemnet.clientWidth、documet.documentElemnet.clientHeight保存了视口信息。ie6中标准模式这些属性有效，混杂模式由document.body.clientWidth/document.body.clientHeight有效。chrome混杂模式下两种方法都可以取得视口大小。
取得视口大小方法：
```
var pageWidth = window.innerWidth,
   	pageHeight = window.innerHeight;
    if (typeof pageWidth != "number"){
    	if (document.compatMode == "CSS1Compat"){
        	pageWidth = document.documentElement.clientWidth;
            pageHeight = document.documentElement.clientHeight;
       	} else {
        	pageWidth = document.body.clientWidth;
            pageHeight = document.body.clientHeight;
        	}
        }
```

+ resizeTo()/resizeBy()：调整窗口大小;参数（新宽度、高度/宽度高度差）

#### 导航和打开窗口
+ window.open():4个参数：加载的url、目标窗口、一个特性字符串、是否取代当前加载页面的历史记录（在不打开新窗口的情况下有效）
+ 第二个参数如果是已有窗口或框架名称，就会在其加载url。如果没有，就会根据第三个参数打开新窗口，调用close()可以关闭新打开的窗口。新建的window对象有一个opener属性（保存打开它的原始对象）。
+ 检测窗口屏蔽程序：
```
var blocked = false;
	try {
    	var wroxWin = window.open("http://www.wrox.com", "_blank");
        if (wroxWin == null){
      	  blocked = true;
        }
    } catch (ex){
        blocked = true;
    }
    if (blocked){
    	alert("The popup was blocked!");
    }
```

#### 间歇调用/超时调用
+ window对象的方法setTimeout():两个参数（执行代码和等待时间（毫秒）），会返回一个ID，可以将id传递给clearTimeout()取消未执行的超时调用
+ setInterval(): 同样会返回一个间歇调用id，可以使用clearInterval()取消。

#### 系统对话框
+ alert()：弹出窗口具有确认按钮
+ confirm()：弹出对话框具有确认/取消按钮
+ prompt()：弹出对话框具有文本输入框
+ window.print()/window.find()显示打印/查找对话框（就像通过浏览器菜单的“查找”“打印”一样）

#### location对象
location提供了与当前窗口中加载的文档的有关信息及一些导航功能，它即是window的属性，又是document的属性。它不但保存了当前文档的信息，还将url解析未独立片段。
> 属性名：hash(#contents),host(服务器名称和端口号)，hostname，href（完整url，location的toString()也是这个值），pathname（返回url中的目录或文件名），port（端口号），protool（使用协议），search（url查询字符串，以问号开头）

+ 查询字符串参数
```
        function getQueryStringArgs(){
            //取得查询字符串并去掉开头的问号
            var qs = (location.search.length > 0 ? location.search.substring(1) : ""),
                //保存数据对象
                args = {},
                //取得每一项
                items = qs.length ? qs.split("&") : [],
                item = null,
                name = null,
                value = null,
                //在for循环中使用
                i = 0,
                len = items.length;
            //将每一项添加到args对象中
            for (i=0; i < len; i++){
                item = items[i].split("=");
                name = decodeURIComponent(item[0]);
                value = decodeURIComponent(item[1]);
                if (name.length){
                    args[name] = value;
                }
            return args;
        }
```

+ 位置操作
> location.assign("url")/window.location="url"/location.href="url":可以打开新的url。修改location的其他属性也可以改变当前加载的页面。（历史记录会生成新记录）。
location.replace("url")可以不生成历史记录。
location.reload()表示重新载入当前页面，如果传入true表示重新加载

#### navigator对象
navigator通常用于检测浏览器类型。
##### 检测插件
+ **非ie浏览器插件检测**
使用plugins数组实现，数组包含:
name：插件的名字；
description：插件的描述；
filename：插件的文件名；
length:插件所处理的MIME类型数量
```
//检测插件（在IE中无效）
function hasPlugin(name){
	name=name.toLowerCase()；
	for (var i=0; i<navigator.plugins .length; i++){
		if (navigator. plugins[i].name. toLowerCase().indexOf (name)>-1)(
			return true；
		}
	}
	return false;
}
//检测Flash
alert (hasPlugin("Flash"));
//检测 QuickTime
alert (hasPlugin("QuickTime"));
//检测Java
alert (hasPlugin( "Java"))；
```

+ **ie浏览器插件检测**
 检测IE中的插件比较麻烦，因为IE不支持Netscape式的插件。在IE中检测插件的唯一方式就是使用专有的ActiveXObject类型，并尝试创建一个特定插件的实例。IE是以COM对象的方式实现插件的，而COM对象使用唯一标识符来标识。因此，要想检查特定的插件，就必须知道其COM标识符。例如，Flash的标识符是ShockwaveFlash．ShockwaveFlash。
```
//检测IE中的插件
function hasIEPlugin (name){
	try{
		new ActiveXObj ect (name)；
		return true；
	} catch(ex){
		return false;
	}
}
//检测Flash
alert (hasIEPlugin("ShockwaveFlash. ShockwaveFlash"))
//检测 QuickTime
alert( hasIEPlugin("QuickTime. QuickTime“));
```

+ **通用检测**
```
//检测所有浏览器中的Flash
function hasFlash()(
	var result=hasPlugin("Flash");
	if(!result){
		result=hasIEPlugin("ShockwaveFlash. ShockwaveFlash")；
	}
	return result;
}
//检测所有浏览器中的QuickTime
function hasQuickTime(){
	var result=hasPlugin("QuickTime")；
	if（!result）{
		result=hasIEPlugin("QuickTime. QuickTime");
	}
	return result;
}
//检测Flash
alert(hasFlash())；
//检测 QuickTime
alert (hasQuickTime())；
```

##### 注册处理程序
firfox2为navigator对象新增了registerContentHandler()和 registerProtocolHandler()方法

#### screen对象
表明客服端能力

#### history对象
history是window对象的属性，每个窗口、标签页、框架都有自己的history对象与特定window对象关联。go()方法可以实现跳转，history.go(-1),也可以使用字符串跳转到包含该字符串的第一个位置。也可以使用back()、forward()实现后退、前进。history.length=0表示实打开窗口的第一个页面。

## 客户端检测
### 能力检测
### 怪癖检测
### 用户代理检测

后续采用分节的方式写javascript-note
