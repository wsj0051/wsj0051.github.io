# HTML5
## 新语义标签
### 网页布局结构标签及兼容处理
[HTML5 语义元素]( http://www.w3school.com.cn/html/html5_semantic_elements.asp)

```html
  <header></header>
  <footer></footer>
  <article></article>
  <aside></aside>
  <nav></nav>
  <section></section>
```
### 多媒体标签及属性介绍
[HTML 视频](http://www.w3school.com.cn/html5/html_5_video.asp)

+ `<video></video>` 视频
	- 属性：controls 显示控制栏
	- 属性：autoplay 自动播放	
	- 属性：loop  设置循环播放
+  `<audio></audio>`  音频
	-  属性：controls 显示控制栏
	-  属性：autoplay 自动播放	
	- 属性：loop  设置循环播放

```html
	 <video>
		<source src="code/多媒体标签/trailer.mp4">
		<source src="trailer.ogg">
		<source src="trailer.WebM">
	</video>
```

## 新表单元素及属性
### 智能表单控件

```html
<input  type="email">
<!-- 
 email: 输入合法的邮箱地址
 url：  输入合法的网址
 number： 只能输入数字
 range： 滑块
 color： 拾色器
 date： 显示日期
 month： 显示月份
 week ： 显示第几周
 time：  显示时间
-->
```

### 表单属性

+ form属性：	
	- autocomplete=on | off          自动完成
	- novalidate=true | false        是否关闭校验

+ input属性：
  - *autofocus  ： 自动获取焦点
  - form：	
  - multiple：	 实现多选效果
​  - *placeholder ： 占位符  （提示信息）
​  - *required：    必填项	
  - list
    ```html
    <input type="text" list="abc"/>
    <datalist id="abc">
        <option value="123">12312</option>
        <option value="123">12312</option>
        <option value="123">12312</option>
        <option value="123">12312</option>
    </datalist>
    ```

​   

## API
### 获取页面元素及类名操作和自定义属性
```javascript
//选择器： 可以是css中的任意一种选择器
//通过该选择器只能选中第一个元素。   
document.querySelector("选择器")；
   
//与document.querySelector区别： querySelectorAll 可以选中所有符合选择器规则的元素，返回的是一个列表。				  //querySelector返回的只是单独的一个元素
document.querySelectorAll("选择器");
	     
Dom.classList.add("类名"); //给当前dom元素添加类样式

Dom.classList.remove("类名"); //给当前dom元素移除类样式

classList.contains("类名"); //检测是否包含类样式

classList.toggle("active");  //切换类样式（有就删除，没有就添加）

```

### 自定义属性

+ data-自定义属性名在标签中，以data-自定义名称  
  - 获取自定义属性 Dom.dataset  返回的是一个对象
  - Dom.dataset.属性名  或者  Dom.dataset[属性名[^1]]
  - Dom.dataset.自定义属性名=值  或者  Dom.dataset[自定义属性名]=值；

### 文件读取
```
  FileReader
  	  FileReader			 接口有3个用来读取文件方法返回结果在result中
    	  readAsBinaryString    ---将文件读取为二进制编码
    	  readAsText		   ---将文件读取为文本
    	  readAsDataURL		   ---将文件读取为DataURL

 FileReader 提供的事件模型
  	 onabort	    中断时触发
       onerror	    出错时触发
       onload	    文件读取成功完成时触发
       onloadend	读取完成触发，无论成功或失败
       onloadstart	读取开始时触发
       onprogress	读取中
```
### 获取网络状态

+ 获取当前网络状态
  		 window.navigator.onLine 返回一个布尔值

+ 网络状态事件
  		 1. window.ononline
        		 2. window.onoffline

### 获取地理定位

1. 获取一次当前位置

   ```
   window.navigator.geolocation.getCurrentPosition(success,error);
   1. coords.latitude   维度
   2. coords.longitude   经度    
   ```

2. 实时获取当前位置

    ```
    window.navigator.geolocation.watchPosition(success,error);
    ```

### 本地存储 
+ localStorage：
  + 永久生效
  + 多窗口共享
  + 容量大约为20M
  ```javascript
  window.localStorage.setItem(key,value)  //设置存储内容
  window.localStorage.getItem(key)  		//获取内容
  window.localStorage.removeItem(key)	 //删除内容
  window.localStorage.clear()			//清空内容
  ```
+ sessionStorage：
  + 生命周期为关闭当前浏览器窗口
  +  可以在同一个窗口下访问
  + 数据大小为5M左右
  ```javascript
  window.sessionStorage.setItem(key,value)
  window.sessionStorage.getItem(key)
  window.sessionStorage.removeItem(key)
  window.sessionStorage.clear()
  ```
  
## Canvas

### 绘图方法

```html
<canvas id="myCanvas" width="200" height="100"></canvas>
<script type="text/javascript">
    var ctx=document.getElementById("myCanvas");
    ctx.moveTo(x,y)    //落笔
    ctx.lineTo(x,y)    //连线
    ctx.stroke();	   //描边
    ctx.beginPath();  //开启新的图层
</script>
```

+ 样式： strokeStyle="值"
+ 线宽： linewidth="值"   备注：不需要带单位
+ 线连接方式：   lineJoin: round | bevel | miter (默认)
+ 线帽（线两端的结束方式）：  lineCap: butt(默认值) | round | square 

```javascript
ctx.closePath(); //闭合路径
```

### 渐变方案

线性渐变

```
var grd=ctx.createLinearGradient(x0,y0,x1,y1);         
```

+ x0-->渐变开始的x坐标
+  y0-->渐变开始的y坐标
+  x1-->渐变结束的x坐标
+  y1-->渐变结束的y坐标   

```javascript
grd.addColorStop(0,"black");      //设置渐变的开始颜色
grd.addColorStop(0.1,"yellow");   //设置渐变的中间颜色
grd.addColorStop(1,"red");        //设置渐变的结束颜色
ctx.strokeStyle=grd;
ctx.stroke();
```

+ 备注：渐变的开始位置和结束位置介于0-1之间，0代表开始，1代表结束。中间可以设置任何小数 

径向渐变

  	ctx.createradialGradient(x0,y0,r0,x1,y1,r1);

+ (x0,y0)：渐变的开始圆的 x,y 坐标
+ r0：开始圆的半径
+ (x1,y1)：渐变的结束圆的 x,y 坐标
+ r1：结束圆的半径

### 填充效果

```
  ctx.fill();	      //设置填充效果
  ctx.fillstyle="值"; //设置填充颜色
```

### 非零环绕原则

  绘制一个如下图形

![矩形](assets/1526470782861.png)

+ 非零环绕原则：
  + 任意找一点，越简单越好
  +  以点为圆心，绘制一条射线，越简单越好（相交的边越少越好）
  +  以射线为半径顺时针旋转，相交的边同向记为+1，反方向记为-1，如果相加的区域等于0，则不填充。
  + 非零区域填充

### 绘制虚线

1. 原理：

    设置虚线其实就是设置实线与空白部分直接的距离,利用数组描述其中的关系

    例如： [10,10]  实线部分10px 空白部分10px

    例如： [10,5]  实线部分10px 空白部分5px

    例如： [10,5,20]  实线部分10px  空白5px  实线20px  空白部分10px 实线5px 空白20px....

2. 绘制：
   
    ```javascript
	ctx.setLineDash(数组);
	ctx.stroke();
	ctx.moveTo(100, 100);
	ctx.lineTo(300, 100);
	ctx.setLineDash([2,4]);
	ctx.stroke();
	```
	
	注意： 如果要将虚线改为实线，只要将数组改为空数组即可。

### 绘制动画效果

 绘制一个描边矩形： `content.strokeRect(x,y,width,height) `

 绘制一个填充矩形： `content.fillRect(x,y,width,height)` 

 清除：`content.clearRect(x,y,width,height)`

 实现动画效果： 

1. 先清屏
2. 绘制图形
3. 处理变量

### 绘制文本

绘制填充文本
```
content.fillText(文本的内容,x,y)
```
绘制镂空文本

```
content.strokeText();
```
设置文字大小：
 ```
 content.font="20px 微软雅黑"
 ```

文字水平对齐方式【文字在圆心点位置的对齐方式】
```
content.textalign="left | right | center"
```
文字垂直对齐方式
```
content.textBaseline="top | middle | bottom | alphabetic(默认)"
```
+ 文字阴影效果
  +  ctx.shadowColor="red";  设置文字阴影的颜色
  +  ctx.ShadowOffsetX=值;   设置文字阴影的水平偏移量
  +  ctx.shadowOffsetY=值;   设置文字阴影的垂直偏移量
  +  ctx.shadowBlur=值;      设置文字阴影的模糊度

### 绘制图片


  	 //将图片绘制到画布的指定位置
  	 content.drawImage(图片对象,x,y);
  	 //将图片绘制到指定区域大小的位置  x,y指的是矩形区域的位置，width和height指的是矩形区域的大小
  	 content.drawImage(图片对象,x,y,width,height);
  	 content.drawImage(图片对象,sx,sy,swidth,sheight,dx,dy,dwidth,dheight);

- sx,sy 指的是要从图片哪块区域开始绘制，
- swidth，sheight 是值 截取图片区域的大小
- dx,dy 是指矩形区域的位置，dwidth,dheight是值矩形区域的大小

解决图片绘制到某一个区域的按原比例缩放绘制：绘制宽：绘制高==原始宽：原始高

### 绘制圆弧

 ```
 content.arc(x,y,radius,startradian,endradian[,direct]);
 ```

1. x,y    圆心的坐标
2.  radius 半径
3. startradian   开始弧度
4.  endradian     结束弧度
5.  direct        方向（默认顺时针 false）   true 代表逆时针   

**角度 和 弧度的关系： 角度:弧度= 180:pi**

+ 0度角: 以圆心为中心向右为0角 顺时针为正，逆时针为负

+ 特殊值
  ```
  0度 = 0弧度

  30度 = π/6   (180度的六分之一)

  45度 = π/4   

  60度 = π/3

  90度 = π/2

  180度 = π

  360度 = 2π
  ```
 绘制圆上任意点：	

+ 公式：

  +  x=ox+r*cos( 弧度 )*
  + y=oy+r*sin( 弧度 )

  1. ox: 圆心的横坐标
  2. oy: 圆心的纵坐标
  3. r： 圆的半径

### 平移【坐标系圆点的平移】

```javascript
ctx.translate(x,y);
```

+ 通过该方法可以将原点的位置进行重新设置。
+ translate(x,y) 中不能设置一个值
+ 与moveTo(x,y) 的区别：
  +  moveTo(x,y) 指的是将画笔的落笔点的位置改变，而坐标系中的原点位置并没有发生改变
  +  translate(x,y) 是将坐标系中的原点位置发生改变   



### 旋转【坐标系旋转】

```
ctx.rotate(弧度)
```

### 伸缩

```
 ctx.scale(x,y)
```

 备注： 沿着x轴和y轴缩放 x,y 为倍数  例如： 0.5  1


[^1]: 属性名是不包含data-**