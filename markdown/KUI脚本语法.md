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
- 支持设置自定义变量，自定义变量的名字由小写字母或小写字母+数字组成，data控件中带{}的数据是KUI脚本的自定义变量，在KUI中的使用方法为将data控件中的变量名前面加上"#"直接使用，自定义变量语法格式与宏程序中的宏变量语法格式相同
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