---
title: QSS基础
description: QSS基础
toc: true
authors:
tags:
categories:
series:
date: '2022-03-08'
lastmod: '2022-03-09'
draft: false
weight: 2
---

QSS基础

<!--more-->

## 语法
```css
selector { attribute:value; }
```
&emsp;&emsp;```selector```代表选择器，指明了哪种控件将会受到规则影响。```attribute:value```代表声明语句，其中` attribute `表示属性，` value `表示该属性的值，属性与它的值之间必须以冒号隔开，属性值后面必须以分号结束。


## 选择器
### 通用选择器
&emsp;&emsp;通用选择器(*)表示匹配程序中所有的```widget```，效率较低，尽量减少使用。
```css
* { 
	font:normal 20px "微软雅黑"; 
}
```
---
### 类型选择器
&emsp;&emsp;类名即Widget类名, 由`QObject :: metaObject() :: className()`获取, 类型选择器匹配所有该类以及该类的派生类的对象。
```css
QPushButton { 
	color:blue; 
}
```
&emsp;&emsp;如果想要为相似控件提供统一样式，则可以如下使用：
```css
QAbstractSpinBox { //QSpinBox, QDateTimeEdit, QTimeEdit, QDateEdit
	min-height: 30px;
	max-height: 30px;
	border-width: 1px;
	order-style:solid;
	order-color:gray;
	padding: 0px;
}
```
&emsp;&emsp;注意：当自定义控件在命名空间中（或是一个嵌套类），`QObject::className()`会返回( :: )，与子控件选择器冲突。解决该问题的方法是要将`::`替换为`--`。
```cpp
namespace ns {
	class MyPushButton : public QPushButton {
		// ...
	}
}

// ...
qApp->setStyleSheet("ns-MyPushButton { background: yellow; }")
```
---
### 类选择器
&emsp;&emsp;类选择器与类型选择器相似，不同点在于类选择器不会匹配其派生类的对象。
```css
.QPushButton {
	color:blue;
}
```
---
### ID选择器
&emsp;&emsp;id指的是`objectName`，一般情况下，`objectName`唯一且不要在运行时改变。
```css
#button_1 {
	color:red;
}
.QLabel#button_1 { //CSS中的交集选择器
	color:red;
}
```
&emsp;&emsp;注意：
1. `objectName`大小写敏感
2. "#"与ID之间没有空格
3. 由于`objectName`允许字符串中含有空格，但是由于第2条的影响，为了使用ID选择器，`	objectName`不能含有空格
4. 实际开发建议使用如上的第2种方式，是Qt官方给出的ID选择器格式，增强可读性。
---
### 后代选择器
&emsp;&emsp;在`BaseDialog`匹配的所有对象中, 找到`QPushButton`所匹配的所有后代对象, 并给它们设置样式。
```css
BaseDialog QPushButton {
	min-width: 120px;
	min-height: 40px;
	max-width: 120px;
	min-height: 40px;
	font-size:20px;
	padding: 0px;
}
```
&emsp;&emsp;注意:
1. 后代选择器必须用空格隔开每个选择器，单个选择器可以使用各种类型的选择器。
2. 后代选择器可以通过空格一直延续。
3. Qt中控件的父子关系取决于如何布局。
---
### 子元素选择器
&emsp;&emsp;子元素选择器表示找到指定选择器所匹配的对象中的所有特定直接子元素然后设置属性, 即找到`.QGroupBox`匹配到的对象中的`.QCheckBox`匹配到的直接子元素然后设置属性`color:blue;`
```css
.QGroupBox > .QCheckBox {
	color:blue;
}
```
&emsp;&emsp;注意：
1. 子元素选择器与后代选择器的不同在于只会查找“儿子”，不会查找其他后代。
2. 子元素选择器可以使用各种类型的选择器。
3. 只能有一个">"，不能延续。
4. 使用子元素选择器时，推荐使用类选择器替代类型选择器。
---
### 属性选择器
&emsp;&emsp;匹配特定属性设置样式的选择器。通常情况下, 这里的属性指的是, 使用` Q_PROPERTY `宏所声明的属性, 并且属性类型
要受` QVariant::toString()`支持. 
```css
[objectName="button"] {	// 值为button的objectName属性
	color:red;
}

[objectName|="button"] { // 值以button开头的objectName属性
	color:red;
}

[objectName~="button"] { // 值包含button的objectName属性，且是以空格隔开的button
	color:red;
}
```
&emsp;&emsp;在设置了样式表后，相应的属性值发生了改变，则有必要重新加载样式表
```cpp
style()->unpolish(this); 
style()->polish(this); 
```
---
### 并集选择器
&emsp;&emsp;并集选择器表示，将每个单独选择器匹配的控件放在同一个集合中设置样式。
```css
.QLineEdit, .QcomboBox {
	border: 1px solid gray;
	background-color: white;
}
```
---
### 子控件选择器
&emsp;&emsp;子控件选择器（Sub-Control）表示对类型选择器或类选择器指定的所有控件的子控件设置样式，注意与CSS3的伪元素不同。
```css
QComboBox::down-arrow {
	image:url(:/res/arrowdown.png);
	subcontrol-origin:margin;
}
```
&emsp;&emsp;注意：
1. Sub-Controls始终相对于另一个参考元素来定位。`QComboBox`的`::down-arrow`会默认置于`QComboBox`的Padding右上角，`::down-arrow`默认置于另一个`::drop-down Sub-Control`的中心，`subcontrol-origin`可以改变其默认位置。
2. `width`和`height`属性用来控制Sub-Controls的size，而设置了`image`就隐式设置了size。
3. `position:relative`，允许Sub-Controls从初始化位置便宜。
4. `position:absolute`，使Sub-Controls的positon和size基于参考元素改变。
5. 像 QComboBox 和 QScrollBar 这样的复杂部件，如果sub-control的一项属性是自定义的，那么其他所有的属性跟sub-control也都应该自定义。
---
### 伪类选择器
&emsp;&emsp;表示对类型选择器或类选择器指定的所有控件设置它在指定状态时的样式。
```css
.QPushButton:hover {
	color:white;
}
.QPushButton:hover:checked {
	color:white;
}
```
&emsp;&emsp;注意：
1. `!`表示取反
2. 伪装态可以链接，隐式包含逻辑与
---
### 不使用选择器（不推荐）
```cpp
pBtn1->setStyleSheet("color:green;")
```
&emsp;&emsp;这条语句等价于（假设pBtn1是QPushButton对象的指针）
```cpp
pBtn1->setStyleSheet("QPushButton, QPushBtton *{color:green;}")
```
---
### 选择器匹配规则
```css
Qdialog QComboBox, QLineEdit { //wrong
	border: 1px solid blue;
}

Qdialog>QComboBox, QLineEdit { //wrong
	border: 1px solid blue;
}

Qdialog QComboBox, QDiaLog QLineEdit { //right
	border: 1px solid blue;
}

Qdialog>QComboBox, QDialog>QLineEdit { //right
	border: 1px solid blue;
}
```
&emsp;&emsp;情景：父控件QDiaLog有子控件QComboBox和QLineEdit，如果使用前两种的写法，QComboBox正常，但程序所有的QLineEdit的边框都变成了1px的蓝色实线。基于以上，多个选择器组合使用时，它们会从右到左进行结合，即`Qdialog QComboBox, QLineEdit`，`Qdialog>QComboBox, QLineEdit`。
## QSS特性
### 层叠性
&emsp;&emsp;层叠性用以处理冲突，只有多个选择器匹配到同一个控件时才发生层叠性，两个选择器匹配到同一个按钮，结果是后面的样式覆盖掉前面的。
```css
pBtn1->setStyleSheet("QPushButton{color:green;}")
pBtn1->setStyleSheet(".QPushButton{color:green;}")
```

### 继承性(Qt-Version>=5.7)
&emsp;&emsp;CSS中，如果一个标签的字体和颜色没有显式设置，会自动从父亲获得。而QSS中，控件不会从其父亲继承字体和颜色的设置。
```cpp
qApp->setStyleSheet("QGroupBox{ color: red; }");
qApp->setStyleSheet("QGroupBox, QGroupBox * { color: red; }"); //让孩子也获得其属性
```
&emsp;&emsp;但要注意的是`QWidget::setFont()`可以设置包括孩子在内的字体，`QWidget::setPalette()`可以设置调色板包括孩子在内的调色板。如果想要字体和调色板被孩子继承，可以给QApplication设置`Qt::AA_UseStyleSheetPropagationInWidgetStyles`(Qt5.7加入)属性。
```cpp
QCoreApplication::setAttribute(Qt::AA_UseStyleSheetPropagationInWidgetStyles, true);
```
### 优先级
&emsp;&emsp;当一个控件被多个选择器选中并且设置了相同的属性（值不同）时， 不能仅仅根据设置样式语句出现的先后顺序进行层叠, 那么控件的样式如何确定，于是引出了选择器的优先级问题。 
1. 设置方式的优先级：控件直接设置的样式 > QApplication设置的样式
2. 样式表本身的优先级：一般而言，选择器越特殊，它的优先级越高。也就是选择器指向的越准确，它的优先级就越高。 
	- 继承：最终样式为离目标最近的样式 	
	- 相同选择器：写在后面的会覆盖掉前面的 
	- 不同选择器：Id > 类 > 类型 > 通配符 > 继承 >默认
3. 优先级权重：
	- 计算id选择器数量[=a]
	- 计算类选择器数量+属性选择器数量[=b]
	- 计算类型选择器数量[=c]
	- 忽略子控件选择器
	- 串联上述abc，数字越大权重越大，优先级越高。


## 盒子模型
<center>

![盒模型](https://qtdebug.com/img/qtbook/qss/QSS-BoxModel-1.png)
</center>
<center>

![盒模型](https://qtdebug.com/img/qtbook/qss/QSS-BoxModel-2.png)
</center>


## 属性

&emsp;&emsp;属性即控件的外观样式。
### 背景属性（background）
1. background-color:
	- 取值类型：`Brush`
	- 默认设置border内的背景颜色
2. background-image:
	- 取值类型：`Url`
	- 格式：`url(filename)`
	- 背景图像会覆盖背景颜色
3. background-repeat:
	- 取值：
		+ repeat-x: 水平平铺
		+ repeat-y: 垂直平铺
		+ repeat: 两方向平铺
		+ no-repeat: 不平铺
4. background-position:
	- 取值：
		+ top: 
		+ bottom: 
		+ center: 
		+ left: 
		+ right
	- 对齐方式： 
		+ background-position: 水平对齐方式 垂直对齐方式
5. background-attachment: 
	- 取值
		+ scroll: (default)滚动
		+ fixed: 固定
	- 设置背景图片在带滚动条的控件(QAbstractScrollArea)中是固定在一个位置还是随着滚动条滚动. 
6. background-clip: 
	- 取值:
		+ margin
		+ border
		+ padding
		+ content
	- 作用: 设置背景颜色覆盖的区域, 默认情况下背景只覆盖内边距矩形, 如果没有指定, 默认值为border 
	- background-clip 属性只对背景的渲染区域有关系, 背景图片始终是靠在padding 边上 
7. background-origin
	- 取值:
		+ margin
		+ border
		+ padding
		+ content
	- 作用: 
		+ 与 background-position 和background-image 一起使用, 指明背景图片的覆盖范围矩形, 如果没有指定, 默认为 padding  
8. 背景连写格式：background
```css
background:color image repeat position; // 连写最多包含此4个
```
### 前景属性（color）
&emsp;&emsp;即控件文字的颜色，取值类型：`Brush`。
### 边框属性（border）
1. border-width: 
	- 取值: `(num)px`
2. border-style:
	- 取值: 
		+ dashed
		+ dot-dash
		+ dot-dot-dash
		+ dotted
		+ double
		+ groove
		+ inset
		+ outset
		+ ridge
		+ solid
		+ none
3. border-color:
	- 取值类型：`Brush`
4. border-radius: 
	- 取值: 水平半径 垂直半径
5. border-image: (一般不用)
	- border-image-source: 图片路径，只支持本地和Qt资源路径
	- border-image-slice: 图片切片，单位只能是像素值，数值不必带单位px
	- border-image-repeat: 
		+ stretch
		+ round
		+ repeat
```css
border-image: border-image-source border-image-slice border-image-repeat
```
6. 边框连写格式：border
```css
// 1.
border: width style color;
// 2. 四方向
border-top: width style color;
// 3. 
border-style: top right bottom left;
border-width: top right bottom left;
border-color: top right bottom left;

// 单写: top width 可替换成对应词条
border-top-width: 
```


### 字体属性（font）
1. font-style
	- normal
	- italic
	- oblique
2. font-weight
	- normal
	- bold
3. font-size
4. font-family
5. 字体连写格式：font
```css
font: style weight size family
```

### 文本属性
-  text-align: 只支持`QPushButton`和`QProgressBar`,``` text-align: center center ```
	+ top
	+ bottom
	+ left
	+ right
	+ center  
-  text-decoration
	+  none
	+  underline
	+  overline
	+  line-through
-  padding
-  margin
-  width|height
-  max-width|max-height|min-height|min-height
	+  当最小宽度与最大宽度相等时, 意味着给这个盒子的内容设置了一个固定宽度. 
	+  当最小高度与最大高度相等时, 意味着给这个盒子的内容设置了一个固定高度.
-  outline
	+  是控件有焦点时, 绘制在边框边缘的外围,可起到突出作用,轮廓线不占据控件, 也不一定是矩形. 
	+  `outline:none; // 有焦点时不绘制轮廓 `
## Brush类型
&emsp;&emsp;brush 一般用来设置颜色，有3种取值：`Color`, `Gradient`, `PaletteRole`。  
+ Color  
	- rgb(r,g,b)  
	- rgba(r,g,b,a)  
	- hsv(h,s,v)  
	- hsva(h,s,v,a)  
	- #rgb
- Gradient
	- qlineargradient 线性渐变
	- qradialgradient 径向渐变
	- qconicalgradient 锥形渐变
- PaletteRole
