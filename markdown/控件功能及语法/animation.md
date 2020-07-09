## <span id="animation">animation</span>

- 功能
    - 显示动画(支持动画文件或按顺序显示几张图片)
- 参数
    - 动画文件的名称或者几张图片的名称(需要加小括号)，若是几张图片会按照顺序自动生成动画（文件需放入当前路径下，如不在当前路径需写入完整路径）
    - (起始行，起始列，占几行（默认占1行），占几列（默认占1列）)
    - 动画的两帧或相邻两张图片显示的时间间隔，单位为ms。默认为300ms
- 控件应用分析与举例
此控件用于在自定义界面中显示动画，可以用动画展示加工工艺或向操作者展示操作步骤。例如，下图中的动图展示的是"金属注射成型"的加工工艺

<style>#div{
     width: 600px;
     height: 400px;
     background: url("pictures/animation应用举例背景.png");
     position: relative; 
     }
     #div #pic{
       width:200px;
       height: 140px;
       background: url("pictures/animation应用举例.gif");
       position: absolute;
       top: 50px;
       left: 10px;
     }</style>

<div id="div">
  <img src=pictures/animation应用举例.gif width="500" id="pic">
</div>

```python
animation("金属注射成型.gif", (1, 1, 4, 4), 100)    #显示动画文件"金属注射成型.gif",文件在当前目录下，在1行1列，占4行4列，设定动画中两张图片显示的时间间隔100ms
```