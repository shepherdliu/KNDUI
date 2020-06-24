# KUI是什么？

- 为了提高KND数控系统的开放性，使用户可以根据实际需要，方便、快捷地设计出与特定功能相匹配的专用界面而提供的"自定义界面开发包"，简称为KUI
- 设计思想：
- 优势：

- 开发者根据教程搭建开发环境，成功安装开发包即可进行自定义界面的开发

# 轻松入门

## KUI界面组成

- 自定义界面的开发语言为python，所以界面的开发程序符合python的语法规则，我们提供的界面定制API简单易用，读者可以在python零基础的情况下直接学习使用。但为了更高效的开发、更少的发生基本的语法错误(例如，编程中所有的标点符号都是英文输入法下的符号，否则报错，程序无法运行)，建议对python进行"简单"的学习
- 自定义界面由一个或多个页面组成，通过自定义菜单切换到不同的页面。可以在所有页面定义之前为页面中的控件定义基本属性，包括字体颜色、大小、对齐方式等

###  页面
- 每个页面都有一个id号，页面下可以定制各种控件和菜单(控件、菜单仅为某一页面所有)
- 页面通过设置行数和列数来布局，可以在页中单独定义此页中控件的基本属性

###  控件
- 每个控件都可以有一个id号，用于标识控件，有些控件由于其功能的特殊性必须有id，有些可以没有
- 控件的属性由参数设定，有些有默认值的属性如果想使用默认值可省略相应的参数

###  菜单
- 菜单分布在页面的最下方，通过数控系统的软按键操作
- 有些菜单的功能是独立的，有些菜单要和相应的控件配合使用

## 例子
- 通过下面的例子认识一下自定义界面。下面的文件可以在下载的工具包中找到，名称为"QingSong.py"。在Pycharm中打开并运行此文件，在当前文件夹下的_output文件夹中找到QingSong.kui文件，用U盘输入数控系统中查看界面
```python
# coding: utf-8
from knd_ui import *

# 此自定义界面在数控系统的"位置"页面下
kui_id(位置)
# 此自定义界面根目录菜单名称为"轻松入门"
menu("轻松入门")

# 此页的id为"main"，共有10行，5列，text为此页控件设定了默认属性
with page(10, 5, id="main", text=(绿色, f32x16, 居中)):
    text("这是一个自定义界面", (1, 2, 1, 3), color=绿色)
    text("自定义界面支持多种控件", (2, 2, 1, 3), 黄色)
    text("字体支持多种颜色", (3, 2, 1, 3), 紫色)

    menu("按此处", "AnotherPageID")

# 这是另一个页面，id为"AnotherPageID"，共有5行5列，text为此页控件设定了默认属性
with page(5, 5, id="AnotherPageID", text=(红色, f32x16, 居中)):
    text("自定义界面可以包含多个页面", (3, 2, 1, 3))

# 在当前目录下新建_output文件夹，生成kui文件
generate(__file__)
```

# 控件功能及语法介绍

## 支持控件
- [page 页面布局](#page) 
- [text 静态文本](#text)
- [coor 坐标](#coor)
- [led 指示灯信号量](#led)
- [switch 开关信号量](#switch)
- [teach 宏变量示教轴](#teach)
- [data 数据](#data)
- [picture 图片](#picture)
- [cartoon 动画图片](#cartoon)
- [menu 菜单](#menu)

##  <span id="page">page</span>
- 功能：page函数用于设置此页面的基本属性，包括页的行、列数，此页中text和data控件的默认属性。代码格式上page函数和with联合使用用于分割页。
- 参数
    - 此页包含的行数
    - 此页包含的列数
    - id号（每个page都有自己的唯一id，可以为数字或中英文字符串）
    - 若本页单独对text控件或data控件字体有整体设置，可加入text和data参数，text和data属性仅在本页面有效。若不想单独设置，则下面两个参数可以省略，省略时将采用默认设置。
    	- text参数是一个元组，用（）括起来，可设置字体，颜色和对齐方式
    	- data参数是一个元组，用（）括起来，可设置字体，颜色
- 控件应用分析与举例 ：
```python
#此页面共10行，5列，id为"page",静态字符text控件默认为红色,字体大小为f24x12, 左对齐。数据data控件默认为黄色，字体大小为f24x12，对齐方式为居中。
page(10, 5, id="page", text=(红色, f24x12, 左对齐),data=(黄色, f24x12))
```
- page函数和with联合使用用于分割页举例 
```python
#冒号下面所有缩进4个格的控件函数生成的控件、菜单都属于这一页
with page(10, 5,id="机床调试", text=(红色, f24x12, 左对齐)):
    text("宏变量", (1, 1))      #静态字符text控件占此页的第1行，第1列
    data("#500", (1, 2))       #数据data控件占此页的第1行，第2列
    
#这是另一个页面
with page(4, 5, id="刀架信号"，data=(黄色, f24x12, 居中)):
    text("系统参数", (1, 1))    #静态字符text控件占此页的第1行，第1列
    data("P414", (1, 2))       #数据data控件占此页的第1行，第2列
```

## <span id="text">text</span>

- 功能
    - 显示任意静态文本
- 参数
    - 显示的字符串(包含在单引号' '或双引号" "中的中英文字符)
    - (起始行，起始列，占几行（默认为占1行），占几列（默认为占1列）)
    - 对齐方式(可以为"左对齐"，"居中"，"右对齐"。不设为默认值，如果page函数中设置了本页的默认值，则和page中一样；page中没设置，则为全局默认设置值)
    - 静态文本的颜色（不设为默认值，见上一行）
    - 字体大小（不设为默认值，见上一行）
    - id 控件名称，用于标识控件，可以不填写。若和后面要说明的switch控件联合使用，则可能需要id，详细说明见switch控件

- 控件应用分析与举例：
此控件用于在页面中显示任意静态文本。所有在页面中不会更改的说明性文字、标识性字符都可以使用text控件。此控件可以与其他控件一起使用（通过合适的布局）来对其他控件进行说明。
例如：下图为为滚齿机开发的自定义界面？？？？？？

 <img src="pictures\text控件应用举例.png" alt="新建工程" style="zoom: 100%;" />

```python
text("开料位置", (1, 3), 红色, 居中)  # 字符串“开料位置”显示于其所在页的第1行、第3列中间位置，占1行1列，颜色为红色，字体大小为默认值。
text("信号K", (1, 1, 2, 2), f24x12, id="weq")   # 字符串“信号K”显示于所在页的第1行、第1列，占2行2列，对齐方式和文本颜色都为默认值，字体大小为f24x12，id为weq
```

## <span id="coor">coor</span>

- 功能
    - 显示机床某个轴的坐标数据
- 支持的数据
    - 绝对坐标和机床坐标 XYZ…
- 参数
    - 坐标描述字符(坐标地址，如X)
    - (起始行，起始列， 占几行（默认占1行）,占几列（默认占1列）)
    - 坐标类型（绝对，机床，默认为绝对坐标）
    - 对齐方式(不设为默认值)
    - 颜色(不设为默认值)
    - 字体大小(不设为默认值)
- 控件应用分析与举例
此控件用于在自定义界面中显示某一个轴的实时坐标，可以为绝对坐标或机床坐标，应该与能说明坐标类型和轴名称的text控件一起使用。
例如：下面的这种常用表现形式，text控件说明坐标类型和轴名，coor控件显示实时坐标

<img src="pictures\coor控件应用举例.BMP" alt="新建工程" style="zoom: 80%;" />

```python
    text("[绝对坐标]", (2, 1))
    text("X", (3, 1))
    coor("X", (3, 2), 绝对)
    text("Y", (4, 1))
    coor("Z", (4, 2), 绝对)
```

## <span id="led">led</span>

- 功能
    - 以指示灯的形式描述K/D/F/G/S/R/X/Y区的位型数据和系统参数的位型数据
- 支持的数据
    - 诊断参数K/D/F/G/S/R/X/Y区的位型数据
    - 系统参数位型数据 格式可为P1105.0-1、P001.1
- 参数
    - 描述的位型数据
    - (起始行，起始列， 占几行（默认占1行），占几列（默认占1列）)
    - 颜色： 默认为绿色和灰色，第一个color为位型信号为1时显示的颜色，第二个color为位型信号为0时显示的颜色
    - id：控件名称，用于标识控件，控件名称不可重复
- 控件应用分析与举例
此控件以指示灯的形式显示系统中的位型数据，相较于0/1的显示方式更加直观，应该与能说明位型数据作用的text控件一起使用

```python
led("X0.0", (7, 4), 绿色, 红色)  # 信号为X0.0,第7行第4列占1行1列,X0.0为1时控件为绿色，为0时控件为红色
```

## <span id="switch">switch</span>

- 功能
    - 以指示灯的形式描述K/D/F区(F区仅支持F5200~F5231)的位型数据和系统参数的位型数据，因为switch控件可以改变位型信号状态，所以仅支持可写的位型数据
- 支持的数据
    - 诊断参数K/D/F区(F区仅支持F5200~F5231)的位型数据
    - 系统参数位型 格式可为 P1105.0-1、P001.1
- 参数
    - 描述的位型数据
    - (起始行，起始列， 占几行（默认占1行），占几列（默认占1列）)
    - 绘制类型：默认为矩形(可以为"圆形"、"矩形")
    - 颜色：默认为绿色和灰色，第一个color为位型数据为1时显示的颜色，第二个color为位型数据为0时显示的颜色
    - text_id：和switch控件一起使用的text控件的id，此时该text控件可以捕获光标。当text_id未写入时要求上一个控件必须为text，以便光标定位使用
    - enable表达式：当该表达式值不为0时该控件才可以修改位型数据，等于0时不可以修改，若不设置该参数，则可以修改
    - id：控件名称，用于标识控件，控件名称不可重复
    - left 左边控件, right右边控件, prev上一控件, next下一控件,当需要设置控件的顺序时设置上下左右控件名称即可
    - action为响应动作，取值仅为"运行脚本"，语法为：action=运行脚本(" xxx.prg")，括号中的字符串可以为任意系统可以识别的G代码程序名称，程序需放在当前文件夹下。当操作者手动改变位型数据的值时，将运行此程序
- 控件应用分析与举例

```python
switch("K0.0", (7, 1),红色, 绿色)   #信号为K0.0,第7行第4列占1行1列,选中是为红色，未选中时为绿色
switch("P0.0", (7, 2))   #信号为系统参数P0.0,第7行第4列占1行1列
switch("K0.1", (7, 3), enable="#11")   # 信号为K0.0,当#11不为0时才可以修改
switch("K0.2", (7, 4), action=运行脚本("O0001.prg")) #当手动改变K0.2的值时将运行程序"O0001.prg"
```

## <span id="teach">teach</span>

- 功能
    - 将轴的坐标(绝对/相对)或表达式的值赋给宏变量
- 参数
    - 被赋值宏变量 #
    - 描述的坐标地址（如X)或表达式（如F1000[u32]/1000）
    - (起始行，起始列， 占几行（默认占1行），占几列（默认占1列）)
    - 颜色
    - 字体大小
    - 线框格式：默认为不隐藏，在此处写入"隐藏"则会隐藏控件的线框
    - enable表达式：当该表达式值不为0时该控件才可以修改或进行示教操作，等于0时控件不捕获光标，故不能操作。若不设置该参数，则可以操作
    - id：控件名称，用于标识控件，控件名称不可重复
    - left 左边控件, right右边控件, prev上一控件, next下一控件(对应数控系统面板上的"左"/"右"/"上"/"下"按键)，当需要设置控件的光标顺序时设置左右上下控件名称即可，4个参数可以设置0个或多个
    - group：控件所属示教组，属于同一示教组的控件可以以组为单位同时示教
- 附加说明：
	此控件的示教功能需要与相应的"菜单"一起使用。详细说明见"menu"中的["示教绝对"](#teach_abs)、["示教机床"](#teach_machine)、["示教表达式"](#teach_expression)、["测量"](#measure)、["加输入"](#add_input)、["部分表达式"](#part_expression)控件
- 控件应用分析与举例
将值赋值给宏变量要干什么？
```python
teach("#530", "X", (4, 1), group=2) #示教时将X轴坐标(机床坐标还是绝对坐标取决于menu控件)赋给宏变量#530，控件在第4行第1列
teach("#531", "F156[s16]/100", (4, 2), enable="D10") #示教时计算表达式F156[s16]/100的值赋给宏变量#531，在第4行第2列。当D10不为0时才可以示教，否则控件无法捕获光标
teach("#533", "Z", (4, 4), group=2) #此控件与第1行的控件同属于一个示教组(group=2)。当菜单示教group=2的按键响应时，两个控件会被同时示教
teach("#540", "#540 + $m", (4, 3)) #此示教控件需要和"测量"、"加输入"、"部分表达式"菜单控件一起使用，详见相应菜单的说明
```

## <span id="data">data</span>

- 功能
    - 显示、修改系统数据和KUI脚本的自定义变量
- 支持的数据
    - 诊断参数 K/D/T/C/X/Y/R/S/F/G，支持位型/字节型参数
    - 宏变量 #
    - 系统参数 格式可为P414， P415-1，P1105.1-1
    - 表达式 如(K12[u8]+#500)/2，会显示诊断参数K12加上#500后除以2得到的值
    - KUI脚本的自定义变量，自定义变量名称可以为字母或字母+数据，为了区别于系统数据，将自定义变量包含在大括号{}内，形如{v1}
- 参数
    - 描述的数据(诊断参数的非位型数据可以设置数据类型，共6种，u8/u6/u32表示无符号数，s8/s6/s32表示有符号数，默认为u8)
    - (起始行，起始列, 占几行（默认占1行）,占几列（默认占1列）)
    - 颜色
    - 字体大小
    - 读写模式：默认是"确认"模式(可以为"只读"或"确认"，表达式无法被赋值，只能是"只读"的)
    - 线框格式：默认为不隐藏，在此处写入"隐藏"则会隐藏控件的线框
    - enable表达式： 当该表达式值不为0时该控件才可以修改，等于0时控件不捕获光标，故不能修改。若不设置该参数，则可以修改(此参数仅在"确认"模式下有用)
    - id 控件名称，用于标识控件，控件名称不可重复，如果不需要可以不设置
    - choices参数为对data控件的扩展，语法为choices={54: "这是G54"，55: "这是G55"}，大括号中每一个被逗号分隔的单元都应该是一个"键--值对"，冒号前面的为"键"，后面的为"值"，大括号中可以包含若干个"键--值对"
    	- 当模式为只读时，控件显示与数据相等的"键"对应的"值"，当数据不与任何"键"相等时显示数据的值
    	- 当模式为确认时，控件显示一个选择文本框，文本框包含大括号里面所有的"值"，选择某个"值"会将相对应的"键"值赋给数据，若参数action有效会同时响应相关动作。数据和大括号里面的所有"键"值都不相等时文本框显示"无效"两个字
    - action为响应动作，可以通过设置action参数实现为data控件输入值时执行适当的动作，action取值可以为"运行脚本"、"运行脚本文件"、"运行MDI"
        - 运行脚本：语法为：action=运行脚本("xxx")，括号中的字符串为符合KUI脚本语法的若干语句。当操作者手动改变控件数据值时，将执行这些语句
        - 运行脚本文件：语法为：action=运行脚本文件("xxx.prg")，括号中的字符串为KUI脚本文件名称，文件需放到当前目录下。当操作者手动改变控件数据值时，将执行此文件
        - 运行MDI：语法为：action=运行MDI，需要与choices参数联合使用，在选择文本框中选择相应"值"时会在MDI模式下执行"值"字符串，如果不符合语法将报错
    - left 左边控件, right右边控件, prev上一控件, next下一控件(对应数控系统面板上的"左"/"右"/"上"/"下"按键)，当需要设置控件的光标顺序时设置左右上下控件名称即可，4个参数可以设置0个或多个
- 控件应用分析与举例


```python
data("#530", (1, 1), f24x12, 只读)    #宏变量#530只读，位于第1行第1列，字符大小为f24x12
data("K200", (1, 2), f20x10, 确认, enable="#100")    #PLC参数k200可写，写入时需确认，当#100不为0时才可以修改
data("D12[u16]*2", (1, 3), f20x10)   #表达式D12*2，第1行第3列，字符大小为f20x10
data("{v2}", (1, 4), f20x10)   #变量名为v2，第1行第4列，字符大小为f20x10,可将此变量用于KUI脚本文件或语句中
data("{AL}", (3, 1), f20x10)   #变量名为AL，第3行第1列，字符大小为f20x10,可将此变量用于KUI脚本文件或语句中
data("D1", (3, 2), 只读, choices={54: "这是G54", 55: "这是G55", 56: "这是G56", 57: "这是G57"})  #以只读方式显示数字对应的字符串，当D1=54时，显示字符串"这是G54"，依此类推，当D1不等于任意"键"时将显示D1的值
data("#500", (3, 3), 浅灰色, f24x12, choices={54: "G54", 55: "G55", 56: "G56", 57: "G57"}, action=运行MDI)  #以选择框的方式显示#500对应的字符串，#500与某一"键"相等时显示对应的"值"，不与任何"键"相等时显示"无效"。通过选择框的下拉选项选择某一"值"时将对应的"键"值赋给#500，并在MDI模式下执行"值"。选择"G54"时将切换到工件坐标G54
data("#501", (3, 4), action=运行脚本文件("O0001.prg")) #当手动改变宏变量#501的值时将执行文件"O0001.prg"中的语句，语句中可以包含data控件中的"v2"和"AL"，语法见KUI脚本语法
```
- 注
    - 诊断参数可以通过Kxx[type]来设置，如K8[u8],表示K区8号参数，无符号8位，这时参数type数据类型会被忽略

## <span id="picture">picture</span>

- 功能
    - 显示图片
- 参数
    - 图片的名称（该图片需放入当前路径下，如不在当前路径需写入完整路径）
    - (起始行，起始列, 占几行（默认占1行）,占几列（默认占1列）)
- 控件应用分析与举例
此控件用于在自定义界面中显示一张图片，可以为？？？？？？。还可以用此控件对界面进行美化，详见"界面美化方案"制作成链接？？？？？？？
```python
picture("75.jpg", (1, 1, 2, 2)#显示图片文件75.jpg(图片位于当前路径下)，占2行2列
picture("D:\\pycharm_projects\平面磨.bmp",(3, 3, 2, 2))
```
- 附加说明：
自定义界面不会改变图片原本的长宽比(将导致图片变形)，所以某些时候可能会发生图片不能充满指定区域的情况，那是因为某个方向已经等于其指定区域的长度，由于固定的长宽比另一个方向长度固定，导致不能充满，此时要重新设计图片。

## <span id="cartoon">cartoon</span>

- 功能
    - 显示动画(支持动画文件或按顺序显示几张图片)
- 参数
    - 动画文件的名称或者几张图片的名称(需要加小括号)，若是几张图片会按照顺序自动生成动画（文件需放入当前路径下，如不在当前路径需写入完整路径）
    - (起始行，起始列，占几行（默认占1行），占几列（默认占1列）)
    - 动画的两帧或相邻两张图片显示的时间间隔，单位为ms。默认为300ms
- 控件应用分析与举例
此控件用于在自定义界面中显示动画，动画？？？？？？？？

```python
cartoon(("1.png","2.png"), (1, 1, 2, 2))    #由图片文件1.png和2.png形成动画，占2行2列，两张图片显示的时间间隔为默认值300ms
cartoon("run.gif", (3, 3, 2, 2)，600)        #读取run.gif动画文件，占2行2列，动画两帧显示的时间间隔为600ms
```

## <span id="menu">menu</span>

- 功能
    - 自定义界面菜单
- 参数
    - 菜单名称
    - Action菜单动作
        - [示教绝对](#teach_abs)
        - [示教机床](#teach_machine)
        - [示教表达式](#teach_expression)
        - [测量](#measure)
        - [加输入](#add_input)
        - [部分表达式](#part_expression)
        - [表达式](#expression)
        - [页面跳转](#goto_page)
        - [页面选择](#select_page)
        - [工件坐标](#work_coor)
        - [信号翻转](#signal_overturn)
        - [生成程序](#gen_prog)
    - enable表达式：当该表达式值不为0时，菜单才显示深色(可以响应按键)否则显示灰色(不响应按键)，不设置enable则无影响
- 语法与功能详解
下面1~6号菜单与teach控件联合使用
<span id="teach_abs">1. 示教绝对</span>：单轴示教时直接写入"示教绝对"，当光标停留在示教坐标数据的teach控件上时，菜单显示深色(可以响应按键)，按下菜单时执行示教绝对坐标的动作。示教轴组时写入"示教绝对(group=x)"，x为相应的轴组号。
```python
teach("#530", "X", (4, 1), group=2)
teach("#531", "Y", (4, 2))
teach("#533", "Z", (4, 3), group=2) 
menu("示教组2", 示教绝对(group=2) #光标停留在1/3行对应的控件上时，此菜单可以响应按键。键按下时同时将#530和#533赋值为X/Z轴的绝对坐标值
menu("示教", 示教绝对)  #光标停留在以上任意一个控件上时，此菜单可以响应按键。键按下时执行相应控件的示教绝对坐标动作
```
<span id="teach_machine">2. 示教机床</span>：单轴示教时直接写入"示教机床"，当光标停留在示教坐标数据的teach控件上时，菜单显示深色(可以响应按键)，按下菜单时执行示教机床坐标的动作。示教轴组时写入"示教机床(group=x)"，x为相应的轴组号。
```python
teach("#530", "X", (4, 1), group=2)
teach("#531", "Y", (4, 2))
teach("#533", "Z", (4, 3), group=2) 
menu("示教组2", 示教机床(group=2) #光标停留在1/3行对应的控件上时，此菜单可以响应按键。键按下时同时将#530和#533赋值为X/Z轴的机床坐标值
menu("示教", 示教机床)  #光标停留在以上任意一个控件上时，此菜单可以响应按键。键按下时执行相应控件的示教机床坐标动作
```
<span id="teach_expression">3. 示教表达式</span>：示教单个表达式时直接写入"示教表达式"，当光标停留在示教表达式的teach控件上时，菜单显示深色(可以响应按键)，按下菜单时执行示教表达式的动作。示教表达式组时写入"示教表达式(group=x)"，x为相应的表达式组号。
```python
teach("#530", "F156[s16]/100", (4, 1), group=2)
teach("#531", "F1805[u32]/1000", (4, 2))
teach("#533", "P414", (4, 3), group=2) 
menu("示教组2", 示教表达式(group=2) #光标停留在1/3行对应的控件上时，此菜单可以响应按键。键按下时同时将#530和#533赋值为对应表达式的计算结果
menu("示教", 示教表达式)  #光标停留在以上任意一个控件上时，此菜单可以响应按键。键按下时计算表达式的值赋给宏变量
```
在以下3个菜单中定义2个变量$m和$v。$v为当前示教控件的编辑值，$m与$v的关系由菜单的类型决定。3个菜单都要与形如teach("#530"，"#530 + $m"，(4,  1))的示教控件联合使用，由于需要使用编辑值所以菜单要在控件有编辑值时才能响应按键
<span id="measure">4. 测量</span>：在测量菜单中$m= -$v，按下菜单键时将当前控件值减去控件的编辑值作为控件的新值，类似测量功能
```python
teach("#530", "#530 + $m", (4, 1))
menu("测量", 测量) #菜单名称为"测量"，当光标停留在上面的teach控件上，并输入了编辑值2时此菜单可以响应按键。键按下时相当于执行#530=#530-2
```
<span id="add_input">5. 加输入</span>：在加输入菜单中$m= $v，按下菜单键时将当前控件值加上控件的编辑值作为控件的新值，类似 "+输入" 功能
```python
teach("#530", "#530 + $m", (4, 1))
menu("+输入", 加输入) #菜单名称为"+输入"，当光标停留在上面的teach控件上，并输入了编辑值2时此菜单可以响应按键。键按下时相当于执行#530=#530+2
```
<span id="part_expression">6. 部分表达式</span>：在部分表达式中$m和$v的关系由表达式决定，$m="表达式"，按下菜单键时将当前控件值加上表达式计算结果值作为控件的新值
```python
teach("#530", "#530 + $m", (4, 1))
menu("测试", 部分表达式("$v - 3")) #菜单名称为"测试"，当光标停留在上面的teach控件上，并输入了编辑值2时此菜单可以响应按键。键按下时相当于执行#530=#530+2-3
```
<span id="expression">7. 表达式</span>：参数为一个赋值表达式，如"#500=#501+1"
```python
teach("表达式", 表达式("#500=#501+1"))  #菜单名称为"表达式"，键按下时执行赋值动作
```
<span id="goto_page">8. 页面跳转</span>：直接写入页面id，按菜单时会跳转到相应页面
```python
with page(5, 4, id="page1"):
	······
	
with page(5, 4, id="page2"):
	······
    menu("切换page1"，"page1") #菜单名称为"切换page1"，键按下时切换到id="page1"的页面

```
<span id="select_page">9. 页面选择</span>：可以根据某一PLC参数的值跳转到相应页面。语法举例：页面选择("K100",("page1", "page2", "page3"))。当K100(此处仅支持PLC参数)的值为0/1/2时分别跳转到id为page1/page2/page3的页面。此时PLC参数类型为U8，页面最多不要超过64个。当PLC参数的值不对应任何id时将跳转到第一个id对应的页面
真正使用时候的例子？？？？？？？？
```python
with page(5, 4, id="page1"):
	······
	
with page(5, 4, id="page2"):
	······

with page(5, 4, id="page3"):
	······
	
with page(5, 4, id="page4"):
	······
menu("页面选择"，页面选择("K100",("page1", "page2", "page3"))  #菜单名称为"页面选择"，当K100(此处仅支持PLC参数)的值为0/1/2时分别跳转到id为page1/page2/page3的页面
```
<span id="work_coor">10. 工件坐标</span>： 切换到相应的工件坐标系。语法举例：工件坐标("G55")
```python
menu("切换G55", 工件坐标("G55")) #菜单名称为"切换G55"，键按下时执行切换到工件坐标系G55的操作
```
<span id="signal_overturn">11. 信号翻转</span>：将相应地址的位信号翻转(在0/1之间转换)，支持的数据与switch控件相同
```python
menu("翻转", 信号翻转("F5231.1")) #菜单名称为"翻转"，键按下时将F5231.1翻转
```
<span id="gen_prog">12. 生成程序</span>：第一个参数为程序模板文件名称（同一路径下），第二个参数为生成的程序号。生成程序时要打开程序开关，否则生成程序失败。在点击"生成程序"时程序模板文件中使用到的变量要有值，否则会报错。
真正使用时的例子
```python
menu("程序", 生成程序("O0002.prg", 12))  #菜单名称为"程序"，KUI脚本程序O0002.prg在当前目录下。在系统端按下此菜单时将以O0002.prg为模板生成程序"O0012.PRG"
```

## 控件属性及缩写

### 支持的字体大小

 - 字体大小，标准格式为Font.sxx, 例如Font.s16x8，缩写格式为f16x8
 - 支持的所有字体大小：f16x8、f20x10、f18x12、f24x12、f24x16、f28x14、f32x16、f32x24、f40x30、f48x36、f64x48、f80x60、f96x72
 - 注：大字体不支持中文，f32x24以上不支持中文

### 颜色

 - 颜色，英文书写格式为Color.xxx, 例如Color.white，同时支持中文
 - 常用颜色: 白色(Color.white) 、 黑色(Color.black)、 红色(Color.red)、黄色 (Color.yellow)、绿色(Color.green)、蓝色 (Color.blue) 、青色(Color.cyan)、紫色（Color.purple）、深灰色 (Color.deep_gray)、灰色(Color.gray)、浅灰色 (Color.tint_gray)、边界白色(Color.border_white)、浅绿色 (Color.tint_green)
 - 上面列出的颜色为常用颜色，如不能满足要求可根据RGB值自定义颜色，参数格式(248, 248, 248)，依次为RGB的值

### 对齐方式

- 对齐方式，英文书写格式为Align.xxx，例如Align.right，同时支持中文
- 支持的所有对齐方式：居中(Align.center)、左对齐(Align.left)、右对齐(Align.right)

### 读写模式

- 读写模式，英文书写格式为Mode.xxx, 例如Mode.ro，同时支持中文
- 支持的所有读写模式：只读(Mode.ro)、确认(Mode.confirm)

### 字节类型

- 字节类型，标准格式为Type.xxx, 例如Type.s8，缩写格式为s8
- 支持的所有字节类型： s8、u8、s16、u16、s32、u32

### switch控件的绘制类型

- switch控件的绘制类型，英文书写格式为BoolType.xxx，例如BoolType.rectangle，同时支持中文
- 支持的所有绘制类型： 矩形（BoolType.rectangle）、圆形（BoolType.circle）

### 坐标类型

- 坐标类型，英文书写格式为Coor.xxx, 例如Coor.abs，同时支持中文
- 支持的所有坐标类型：绝对(Coor.abs)、机床(Coor.mt）

### 菜单动作

- 菜单动作，英文书写格式为Action.xxx，例如Action.teach_abs，同时支持中文
- 支持的所有菜单动作：示教绝对（Action.teach_abs）、示教机床（Action.teach_mt）、示教表达式（Action.teach_expr）、测量（Action.measure）、加输入（Action.add_input）、部分表达式（Action.part_expr）、表达式（Action.expr_control）、页面选择（Action.s_page）、页面跳转（Action.page）、工件坐标（Action.workpiece）、信号翻转（Action.overturn）、生成程序（Action.gen_prog）

### 控件动作

- 控件动作，英文书写格式为WidgetAction.xxx, 例如WidgetAction.run_mdi表示运行MDI程序，同时支持中文
- 支持的所有控件动作：运行MDI（WidgetAction.run_mdi）、运行脚本(WidgetAction.run_script_stream)、运行脚本文件(WidgetAction.run_script_file)

### 线框格式
- 线框格式，英文书写格式为LineShow.xxx，例如LineShow.show，同时支持中文
- 支持的所有线框格式：隐藏(LineShow.hide)、LineShow.show

## 设置默认颜色、字体大小和布局

- 设置所有控件默认颜色、字体大小和布局
```python
default_attr(白色, f24x16, 左对齐)       #设置所有控件默认颜色为白色，字体大小为f24x16，对齐方式为左对齐
```
- 设置静态字符默认颜色、字体大小、对齐方式
```python
default_text_attr(红色, f24x12, 居中)  #表示所有静态文本默认颜色为红色，字体大小为f24x12,对齐方式为居中
```
- 设置数据值默认颜色和字体大小
```python
default_data_attr(黄色, f20x10)   #表示所有数据的默认颜色为黄色，字体为f20x10
```

## 页面是否显示标题栏？？？？？？？？换函数名称

- title参数取值可以为True和False(默认为False)，为True时自定义界面的标题栏不显示，界面最大化；为False时显示标题栏
```python
set_size(width, height, titile=True) #目前支持800X480,600X480,800X600, 默认为800X600，不显示标题栏
```

## 表达式规则

- 表达式操作符支持加减乘法除和小括号"+"、"-"、"*"、"/"、"()"。
- 表达式支持的变量为坐标、系统参数、PLC参数、宏变量
- 通过中括号"[]"来表示地址的属性，如D100[u8]，表示PLC参数D100,D100的属性为u8。
- 需要描述的属性为：
    - PLC参数属性u8、s8、u16、s16、u32、s32, 如D100[u8]，表示PLC参数D100,D100的属性为u8。 
    - 坐标属性abs、mt,如X[abs], 表示X绝对坐标
    - 由于XYC地址既可以做PLC参数，也可以做坐标轴，使用XYC地址时必须加入属性
- 表达式举例：#500+X[abs]*2-K10[u8]
- plc赋值表达式表达式溢出时会自动强制转换为当前的类型，如F5210[u8]=F5210[u8]+ 1,当值超过255时，F5210为8位，会转为0
当#530=F5210[u8]=F5210[u8]+ 1,F5120[u8]溢出时，#530也会被赋值为溢出后的值

## 网络传输和U盘传输

生成的自定义界面文件可以通过网线直接传输或者U盘拷贝的方式输入数控系统中
- 网络传输
    - 查看数控系统IP地址，将开发自定义界面的计算机和数控系统的IP地址调到同一网段，用网线连接
    - 在数控系统中将系统参数P6.2置为1
    - 将download函数写在文件的末尾，语法如下
```python
download(__file__, "xxx") # 生成kui文件并下载到数控系统中，xxx为数控系统IP地址, 默认控件顺序为纵向(按系统操作面板中的"上", "下"按钮光标移动方向)，可设置参数horizontal=True设置控件顺序为横向
download(__file__, xxx, horizontal=True) # 生成kui文件控件顺序为横向
```
- U盘传输
	- 将generate函数写在文件的末尾，语法如下
	- 将在当前文件夹的目录下的output文件夹中生成对应的__xxx__.kui文件(xxx为当前python文件名称), 将此文件拷入U盘，再拷入数控系统中
```python
generate(__file__)  #如果需要控件按照横向排序请在参数中加入horizontal=True
```

## 增加F区写入功能

- F区写入范围为F5200~F5231

## v6多通道宏变量和别名支持

- 多通道宏变量为#540@Pxx，xx为通道号，省略@Pxx时为通道1
- 辅助通道宏变量为#540@Axx，xx为辅助通道号
- 可以输入系统别名宏变量如#ABSIO[1],系统支持的宏变量见V6多通道补充说明，也可在后面加入@Pxx或@Axx表示通道及索引

## KUI脚本语法

KUI脚本语法与宏程序语法基本相同，不同之处在下面列出
- 支持功能更丰富的IF语句：IF、ELSEIF、ELSE、ENDIF
```
    IFxxxTHEN;
    xxx
    ELSEIFxxxTHEN;
    xxx
    ELSE
    xxx
    ENDIF;
```
- WHILE语句格式更改为由ENDWHILE结束，WHILE语句可以嵌套使用，ENDWHILE与上边距离最近的未组合的WHILE  DO组合
```
WHILExxxDO;
xxx
ENDWHILE;
```
- 支持FOR循环，语法为：
```
FOR a=b TO c BY d DO; // a只能是一个自定义变量或宏变量中的局部变量，
xxx     //[b,c]是一个闭区间。d为变量a每次的递增值默认为1，可正、可负也可以是浮点数
ENDFOR; // 当a不在[b,c]区间之内时结束循环

FOR #a1= 1 TO 20 DO;
xxx              //中间代码循环20次
ENDFOR;

FOR #1= 10 TO 1 BY -1.9 DO;
xxx              //中间代码循环5次
ENDFOR;
```
- 支持设置自定义变量，自定义变量的名字由字母或字母+数字组成，data控件中带{}的数据是KUI脚本的自定义变量，使用方法为将data控件中的变量名前面加上"#"直接使用，自定义变量语法格式与宏程序中的宏变量语法格式相同
```python
data("{v2}", (1, 2))  #自定义变量为v2，在KUI脚本中名称为#v2，#v2的使用语法与宏程序中宏变量相同
```
- KUI脚本支持设置和读取数控系统数据(系统参数、宏变量、PLC参数)
```
#103=#["P1115"]         //读取系统参数P1115到#103
#["P1105.5-3"]=1        //系统参数P1105.5-3设置为1 
#500=#["D5"]            //读取D5的值到#500
#["K0.3"]=1             //K0.3设置为1
```
- EXPORT函数，输出数控系统的参数到U盘，目前只支持输出PLC配置参数，语法为：
```python
menu("执行脚本", 运行脚本（"EXPORT[\"plccfg\"]")  #菜单名称"执行脚本"，键按下时向U盘输出PLC配置参数
```
- DEG函数，弧度值转角度值
- RAD函数，角度值转弧度值

- 不支持GOTO
- 不支持OPENWRITE、CLOSEWRITE、WRITE
- 不支持位操作，如<<、>>、~、&、^、XOR、|
- 不支持CALL和PARAMS

## 多页面支持

- 使用函数kui_id（xxx）指定kui页面在数控系统中的位置，参数xxx的全部取值为：位置（PageType.pos）、程序（PageType.prog）、刀补1（PageType.ofs1）、刀补2（PageType.ofs2）、设置1（PageType.set1）、设置2（ PageType.set2）、梯图（ PageType.ladder），中英文都支持。不同位置的自定义界面的开发代码需要放在独立的python文件中

## 支持的系统版本
- 不同的系统版本生成的自定义界面会略有不同，所以要针对系统版本生成对应的自定义界面
- 格式为 版本号_BUILD号 ， 支持的系统版本为V5.1.00c，V6.1.00c，V5.1.01c，BUILD号可以在系统的维护页面看到
```python
sys_version("V5.1.00c_18728") #生成的自定义界面适用于对应的数控系统
```

## 注意事项
- 自定义界面编程语言为pyhon，在编写界面代码时除汉字外的所有符号都应该在英文输入法下输入。当输入中文符号时Pycharm会有红色的语法错误提示，运行时报错，可以根据提示信息快速定位出错位置。

## demo
- 这里对控件、菜单中较为复杂的部分进行举例说明，主要包括生成程序、示教组、加输入、测量、部分表达式、data控件中的显示字符串功能和运行MDI功能。并在界面中用文字对控件的使用进行了说明。在前面下载的"工具包"中有此文件，名称是"demo.py"
```python
# coding: utf-8
from knd_ui import *

# 
set_size(800, 600, title=True)

# 此处为自定义界面中的控件设定一些默认设置，良好的默认设置可以简化后面的控件定义
# 在定义页面之前设置所有控件默认字体颜色，字体大小，对齐方式
default_attr(白色, f24x16, 左对齐)
# 设置所有页面中的静态字符控件默认字体颜色，字体大小，对齐方式
default_text_attr(紫色, f24x16, 居中)
# 设置所有页面中的数据控件默认字体颜色，字体大小
default_data_attr(黄色, f20x10)

# 设置系统型号
# sys_version("V5.1.01c_21288")

# 此自定义界面在数控系统的"位置"页面下
kui_id(位置)
# 此自定义界面根目录菜单名称为"刀架信号"
menu("刀架信号")

# 此页的id为"main"，共有10行，5列。
with page(10, 5, id="main"):
    text("a1", (1, 1))
    text("b1", (2, 1))

    data("{a1}", (1, 2))
    data("{b1}", (2, 2))

    text("#508", (4, 1))
    text("#509", (5, 1))
    text("#510", (6, 1))
    text("#511", (7, 1))

    data("#508", (4, 2))
    data("#509", (5, 2))
    data("#510", (6, 2))
    data("#511", (7, 2))

    text("左侧数据必须都有值才能生成程序", (4, 3, 1, 3), 浅绿色, f24x12)

    menu("Teach", "TeachPageID")
    menu("Data", "DataPageID")
# 生成程序菜单使用了Script.txt为模板，按下按键时生成的程序为O0011.PRG
    menu("生成程序", 生成程序("Script.txt", 11))

with page(5, 5, id="TeachPageID"):
    teach("#501", "X", (1, 2), 浅绿色, group=1)
    teach("#502", "Y", (2, 2), 浅绿色, group=1)
    teach("#503", "Z", (3, 2), 浅绿色, group=1)

    text("左侧三个控件为一个示教组", (2, 3, 1, 3))
    teach("#504", "F1805[u8]/1000", (4, 2))
    text("左侧为一个示教表达式", (4, 3, 1, 3))

    teach("#505", "#505 + $m", (5, 2)) 
    text("左侧控件有编辑值时下方菜单可用", (5, 3, 1, 3))

# 这里演示了示教功能，包括示教组、测量、加输入、部分表达式等
    menu("示教组1", 示教绝对(group=1))
    menu("表达式", 示教表达式)
    menu("测量", 测量)
    menu("+输入", 加输入)
    menu("+双倍", 部分表达式("$v * 2"))

with page(5, 5, id="DataPageID"):
    data("#506", (1, 2))
    data("#506", (2, 2), 只读, choices={54: "这是G54", 55: "这是G55", 56: "这是G56", 57: "这是G57"})

    data("#507", (3, 2), choices={54: "G54", 55: "G55", 56: "G56", 57: "G57"}, action=运行MDI)

    text("左侧控件输入54/55/56/57", (1, 3, 1, 3))
    text("选择的同时切换至相应工件坐标系", (3, 3, 1, 3))
# 在当前目录下新建_output文件夹，生成kui文件
# generate(__file__)

download(__file__, "192.168.7.30")
```

# 进阶+案例

## 界面美化方案

通过常用画图软件（Windows 自带的画图、国产Mockplus软件等），按照像素，对界面进
行设计，如加入图标，图形，表格等。
将设计好的界面保存成图片，用自定义界面中的picture控件将其导入自定义界面中，再加入其它控件配合使用。这里以生成表格为例。

1. 在软件Mockplus中画好表格，也可以在其中加入文字，将需要填入数据的表格空出来，设计好后存成图片。设计表格时表格的位置、宽度、长度等属性要结合自定义界面中能生成的尺寸设计。

<img src="pictures\国产软件设计图.jpg" alt="新建工程" style="zoom: 40%;" />

2. 编辑python文件，在适当位置加入相应的控件组合成自定义界面

<img src="pictures\设计图后.jpg" alt="新建工程" style="zoom: 50%;" />

## 用python简化编程
Python 是一门开源免费、通用型的脚本编程语言，它上手简单，功能强大。可以适当的使用python来简化编程(需要学习python语法)
例1：一个页面的多个控件结构相同，可以通过python的循环控制语句来简化编程。
```python
with page(10, 4, id="a"):
	for bit in range(8):
		text("D0.{0}开关".format(bit), (bit + 1, 1))
		switch("D0." + str(bit), (bit + 1, 2))
		text("X0.{0}信号".format(bit), (bit + 1, 3))
		led("X0." + str(bit), (bit + 1, 4))
```
运行结果如下图所示

<img src="pictures\python循环应用.PNG" alt="新建工程" style="zoom: 50%;" />

例2：将多个控件布局成圆形，可以通过python来计算控件位置
```python
from math import *

def circle(x, y, r, number, a1, b1, data_list):
	for i in range(number):
		xs = int(x + r*cos(i*360/number/57.29578) - a1/2)
		xs = int(x + r*cos(i*360/number/57.29578) - a1/2)
		data(data_list[i], (xs, ys, a1, b1))
		
with page(454, 792, id="a"):
	a = ["#501", "#502", "#503", "#504", "#505", "#506", "#507", "#508", "#509", "#510"]
	circle(200, 200, 120, 10, 30, 30, a)
```
运行结果如下图所示

<img src="pictures\圆形.BMP" alt="新建工程" style="zoom: 50%;" />

如前所述python是一种入门容易，但功能强大的编程语言，这里的例子很简单粗糙，仅作抛砖引玉之用

## 案例

# 附录

## 安装工具软件

- 安装3.6及更高版本的python，具体步骤如下：

	1. 在python官网下载3.6或更高版本的python(建议下载最新版本)，根据计算机硬件下载32位或64位版本。
	
	2. 安装python时，勾选Add Python 3.6 to PATH(将python3.6.8放入系统路径中)，然后点击Install Now，如下图
	
	   ![2](pictures\InstallSoftwares\python.png)
	
	3. 随后按默认设置点击安装，完成安装
	
- 安装2018.3.2及更高版本的pycharm community edition，具体步骤如下：
	1. pycharm的community版本是免费的，可以直接在官网下载(建议下载最新版本)，双击下载的exe文件进行安装
	
	2. 点击next，按Browse可修改安装路径，如下图：
	
	   ![2](pictures\InstallSoftwares\2.png)

	3. 选择系统的位数，如果64位，选在64-bit launcher，其余不用勾选，点击next，如下图：
	
	   ![3](pictures\InstallSoftwares\3.png)
	
	4. 随后按默认设置点击安装，完成安装。python和pycharm安装完成后重启计算机
	
- 安装开发界面及学习本教程需要使用的"工具包"，包中包括"自定义界面开发包"和本教程中的例子
	- 点击"下载工具包"
	- 点击目录中的""........？？？？？？？？

- 检验以上所有的准备工作是否都成功完成：
  1. pycharm第一次打开时弹出以下对话框，直接点击OK

     ![6](pictures\InstallSoftwares\6.png)

  2. 点击OK后会有如下对话框，直接点击Skip Remaining and Set Defaluts

     ![7](pictures\InstallSoftwares\7.png)

  3. 弹出如下对话框，把勾去掉，点击close

     ![8](pictures\InstallSoftwares\8.png)

  4. 点击Create New Project新建工程

     <img src="pictures\InstallSoftwares\新建工程.png" alt="新建工程" style="zoom: 40%;" />

  5. 将工程放在合适的位置，并选择刚刚安装的python作为Interpreter

  <img src="pictures\InstallSoftwares\配置Interpreter.png" alt="新建工程" style="zoom: 40%;" />

  6. 将上面工具包中的test.py文件复制到工程文件夹下，在pycharm中打开文件，在文件页面点击鼠标右键选择"runTest"运行程序。
  
  <img src="pictures\InstallSoftwares\runTest.png" alt="新建工程" style="zoom: 40%;" />
  
  7. 生成如图所示的自定义界面表示成功安装了所有的开发工具。

<img src="pictures\工具安装成功.png" alt="新建工程" style="zoom: 50%;" />