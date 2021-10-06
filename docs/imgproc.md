# 图像处理

## 图形变换

#### 调整图像大小 cv2.resize

函数原型:cv2.resize(源数据,::SIZE对象,x轴缩放倍数,y轴缩放倍数,插值方式) 

> 插值方式常量 _CV2_INTER

| 常量               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| _CV2_INTER_NEAREST | 最近邻插值                                                   |
| _CV2_INTER_LINEAR  | 线性插值                                                     |
| _CV2_INTER_CUBIC   | 双线性插值                                                   |
| _CV2_INTER_AREA    | 使用像素区域关系重新采样。它可能是图像抽取的首选方法，因为它可以提供无莫尔条纹的结果。但是当图像被缩放时，它类似于INTER_NEAREST方法。 |

#### 转换颜色空间 cv2.cvtColor

opencv默认颜色空间为`BGR`,支持的颜色空间有 `RGB`、`HSI`、`HSL`、`HSV`、`HSB`、`YCrCb`、`CIE XYZ`、`CIE Lab` 8种

函数原型:cv2.cvtColor(输入图像,颜色空间代码) 

> 颜色空间代码常量 _CV2_COLOR_



## 平滑处理

### 图像滤波

#### 什么是图像滤波

图像滤波，即在尽量保留图像细节特征的条件下对目标图像的噪声进行抑制，是图像预处理中不可缺少的操作，其处理效果的好坏将直接影响到后续图像处理和分析的有效性和可靠性。（摘自网络）

#### 图像滤波的目的

1. 消除图像中混入的噪声
2. 为图像识别抽取出图像特征

#### 图像滤波的要求

1. 不能损坏图像轮廓及边缘
2. 图像视觉效果应当更好

#### 滤波器的种类

3种线性滤波：`方框滤波`、`均值滤波`、`高斯滤波`

2种非线性滤波：`中值滤波`、`双边滤波`

> 以下提供了图形化滤波函数范例

```aardio
import win.ui;
/*DSG{{*/
var winform = win.form(text="aardio form";right=633;bottom=589)
winform.add(
button={cls="button";text="中值滤波";left=505;top=14;right=611;bottom=50;z=1};
button3={cls="button";text="双边滤波";left=505;top=275;right=611;bottom=311;z=2};
button4={cls="button";text="均值滤波";left=505;top=79;right=611;bottom=115;z=3};
button5={cls="button";text="高斯滤波";left=505;top=145;right=611;bottom=181;z=4};
button6={cls="button";text="方框滤波";left=505;top=210;right=611;bottom=246;z=5};
picturebox={cls="picturebox";left=14;top=14;right=229;bottom=566;z=6};
picturebox2={cls="picturebox";left=263;top=14;right=478;bottom=566;z=7}
)
/*}}*/

import cv2
winform.picturebox.image = "./images/Lena.jpg"
src = cv2.imread( "./images/Lena.jpg" )

winform.button.oncommand = function(id,event){
	var dst = cv2.medianBlur(src,5)
	winform.picturebox2.setBitmap(dst.toBitmap().copyHandle())
}

winform.button4.oncommand = function(id,event){
	var dst = cv2.blur( src,::SIZE(5,5) )
	winform.picturebox2.setBitmap(dst.toBitmap().copyHandle())
}

winform.button5.oncommand = function(id,event){
	var dst = cv2.gaussianBlur( src,::SIZE(5,5) )
	winform.picturebox2.setBitmap(dst.toBitmap().copyHandle())
}

winform.button6.oncommand = function(id,event){
	var dst = cv2.boxFilter( src,-1,::SIZE(5,5) )
	winform.picturebox2.setBitmap(dst.toBitmap().copyHandle())
}

winform.button3.oncommand = function(id,event){
	var dst = cv2.bilateralFilter(src,32,100,200)
	winform.picturebox2.setBitmap(dst.toBitmap().copyHandle())
}

winform.show();
win.loopMessage();
return winform;
```

![](https://i.loli.net/2021/09/30/YXD3oAWTfSZczV6.png)



# 图像形态学

### 形态学操作

形态学操作是指改变图像中图形的形状,一般作用于二值化图像中.二值化图像只有`0` `255`二个值,确切的讲,形态学操作改变的是255值(白色)的图形形状,使之在设定的核形状上"变胖"或"变瘦".

### 1.结构元素

结构元素也叫做核,形态学操作即应用结构元素做卷积.结构元素可以是矩形/椭圆/十字形,可以用`cv2.getStructuringElement`来生成不同形状的结构元素

函数原型:cv2.getStructuringElement(元素形状 默认矩形,元素大小 ::SIZE对象,锚点位置 默认中心点) 

| 常量                      | 形状   |
| ------------------------- | ------ |
| 0/\*_CV2_MORPH_RECT\*/    | 矩形   |
| 1/\*_CV2_MORPH_CROSS\*/   | 十字星 |
| 2/\*_CV2_MORPH_ELLIPSE\*/ | 椭圆   |

```aardio
kernel = cv2.getStructuringElement(0/*_CV2_MORPH_RECT*/, ::SIZE(5, 5))  // 矩形结构
kernel = cv2.getStructuringElement(1/*_CV2_MORPH_CROSS*/, ::SIZE(5, 5))  // 椭圆结构
kernel = cv2.getStructuringElement(2/*_CV2_MORPH_ELLIPSE*/, ::SIZE(5, 5))  // 十字形结构
```

### 腐蚀 cv2.erode

在原图的小区域内取局部最小值.因为是二值化图，只有0和255，所以小区域内有一个是0该像素点就为0

函数原型:cv2.erode(输入图像,结构元素,迭代次数 默认为1) 

### 膨胀 cv2.dilate

膨胀与腐蚀相反，取的是局部最大值

函数原型:cv2.dilate(输入图像,结构元素,迭代次数 默认为1) 

> cv2.erode 与 cv2.dilate 对比范例

```aardio
import cv2
src = cv2.imread("./images/person.jpg",0/*_CV2_IMREAD_GRAYSCALE*/)
src = cv2.threshold( src,220,255,0x1/*_CV2_THRESH_BINARY_INV*/ )
kernel = cv2.getStructuringElement( 0/*_CV2_MORPH_RECT*/,::SIZE(9,9) )
dst = cv2.dilate(src,kernel)
dst2 = cv2.erode(src,kernel)
cv2.imshow( "src",src )
cv2.imshow( "dilate变胖",dst )
cv2.imshow( "erode变瘦",dst2 )
cv2.waitKey(0)
```

![](https://i.loli.net/2021/09/30/uTxI71UNsrgaGPK.png)



![](https://i.loli.net/2021/09/30/5I7HyGEwpfLgkD8.png)

![](https://i.loli.net/2021/09/30/WhlIeF16uft34Ri.png)

###  通用形态学运算

1. 先腐蚀后膨胀叫开运算,其作用是：分离物体，消除小区域

2. 先膨胀后腐蚀叫闭运算,消除"闭合"物体里面的小黑洞
3. 顶帽,原图减去开运算后的图
4. 黑帽,闭运算后的图减去原图
5. 梯度,膨胀图减去腐蚀图,这样会得到物体的轮廓

函数原型:cv2.morphologyEx(输入图像,形态学类别,结构内核,迭代次数 默认为1) 

| 常量                       | 形态学类别 |
| -------------------------- | ---------- |
| 2/\*_CV2_MORPH_OPEN\*/     | 开运算     |
| 3/\*_CV2_MORPH_CLOSE\*/    | 闭运算     |
| 4/\*_CV2_MORPH_GRADIENT\*/ | 梯度       |
| 5/\*_CV2_MORPH_TOPHAT\*/   | 顶帽       |
| 6/\*_CV2_MORPH_BLACKHAT\*/ | 黑帽       |

> 形态学梯度运算

```aardio
import cv2
src = cv2.imread("./images/person.jpg",0/*_CV2_IMREAD_GRAYSCALE*/)
src = cv2.threshold( src,220,255,0x1/*_CV2_THRESH_BINARY_INV*/ )
kernel = cv2.getStructuringElement( 0/*_CV2_MORPH_RECT*/,::SIZE(3,3) )
dst = cv2.morphologyEx( src,4/*_CV2_MORPH_GRADIENT*/,kernel )
cv2.imshow( "梯度运算",dst )
cv2.waitKey(0)
```

![](https://i.loli.net/2021/09/30/qJjHcgkS2XtbyWI.png)



# 阈值分割

阈值处理是一种简单、有效的将图像划分为前景和背景的方法。图像分割通常用于根据对象的某些属性(例如，颜色、边缘或直方图)从背景中提取对象。最简单的阈值方法会利用预定义常数(阈值)，如果像素强度小于阈值，则用黑色像素替换，如果像素强度大于阈值，则用白色像素替换。

### 图像阈值处理 cv2.threshold

函数原型：cv2.threshold(灰度图,起始值,最大值,阈值类型-默认THRESH_BINARY) 

> cv2.threshold也可以做自适应阈值分割，阈值类型为 _CV_THRESH_OTSU

### 自适应阈值二值化 cv2.adaptiveThreshold

函数原型：cv2.adaptiveThreshold(灰度化图像,最大灰度值,自适应阈值算法,,阈值类型,区域大小,常数 可为负数) 



#### 范例

```aardio
import win.ui;
/*DSG{{*/
var winform = win.form(text="aardio form";right=723;bottom=621)
winform.add(
groupbox={cls="groupbox";text="函数选择";left=17;top=38;right=324;bottom=94;edge=1;z=3};
plus={cls="plus";left=11;top=106;right=695;bottom=608;z=2};
radiobutton={cls="radiobutton";text="threshold";left=31;top=63;right=136;bottom=80;checked=1;z=4};
radiobutton2={cls="radiobutton";text="adaptiveThreshold";left=124;top=62;right=275;bottom=86;z=5};
trackbar={cls="trackbar";left=12;top=10;right=291;bottom=40;max=255;min=0;z=1}
)
/*}}*/

winform.trackbar.pos = 127
//拖动滑块,修改图像阈值
import cv2
var img = cv2.imread( "./images/Lena.jpg",0/*_CV2_IMREAD_GRAYSCALE*/ )
scaleX = winform.plus.width / img.width
scaleY = winform.plus.height / img.height
scale = math.min(scaleX,scaleY )
img = cv2.resize(img,,scale,scale)
var bmp,err = img.toBitmap()

winform.plus.setForeground(bmp)
winform.trackbar.oncommand = function(id,event,pos){
	if( event == 8/*_SB_ENDSCROLL*/ ){
		if(winform.radiobutton.checked){
			winform.text = winform.trackbar.pos
			var thresh = winform.trackbar.pos
			src = img.clone()
			var ret,dst = cv2.threshold( src,thresh,255)
			bmp,err = dst.toBitmap()
			winform.plus.setForeground(bmp)
			winform.plus.redraw()			
		}
	}
}

winform.radiobutton2.oncommand = function(id,event){
	if(winform.radiobutton2.checked){
		src = img.clone()
		var dst = cv2.adaptiveThreshold( src,255,1/*_CV2_ADAPTIVE_THRESH_GAUSSIAN_C*/,
			0x0/*_CV2_THRESH_BINARY*/,15,0.0)
		bmp,err = dst.toBitmap()
		winform.plus.setForeground(bmp)
		winform.plus.redraw()
	}
}

winform.radiobutton.oncommand = function(id,event){
	if(owner.checked){
		winform.text = winform.trackbar.pos
		var thresh = winform.trackbar.pos
		src = img.clone()
		var ret,dst = cv2.threshold( src,thresh,255 )
		bmp,err = dst.toBitmap()
		winform.plus.setForeground(bmp)
		winform.plus.redraw()		
	}
}

winform.show();
win.loopMessage();
return winform;
```

![](https://i.loli.net/2021/10/02/HVFSp2hK71ajgtL.png)



# 绘图和注释

OpenCV提供简洁的绘图函数，可以方便的绘制 `线段`、`箭头线段`、 `矩形`、 `圆形`、 `椭圆` 、`水印` 、`文字`等

#### 范例代码

```aardio
import cv2
path = ..io.fullpath("./assets/images/Lena.jpg")
img = cv2.imread(path,1/*_CV2_IMREAD_COLOR*/)

img = cv2.line(img,::POINT(100,100),::POINT(200,200),::Scalar(255,0,255,0),3)
cv2.imshow("line",img)
img = cv2.arrowedLine(img,::POINT(200,250),::POINT(270,550),::Scalar(255,0,255,0),3)
cv2.imshow("arrowedLine",img)

img = cv2.rectangle(img,::POINT(100,100),::POINT(200,200),::Scalar(255,0,255,0),3)
cv2.imshow("rectangle",img)

img = cv2.circle( img,::POINT(150,150),50,::Scalar(255,0,255,0),-1 )
cv2.imshow("circle",img)

cv2.ellipse( img,::POINT(100,50),::SIZE(100,50),0,0,360,::Scalar(0,0,255),3 )
cv2.imshow("ellipse",img)

img = cv2.drawMarker(img,::POINT(100,100),::Scalar(0,0,255),1/*_CV2_MarkerTypes_MARKER_TILTED_CROSS*/,50,10 )
cv2.imshow("drawMarker",img)

img = cv2.putText(img,"你 好 Lena!",::POINT(0,20),,20)
cv2.imshow("putText",img)
cv2.waitKey()
```

![](https://i.loli.net/2021/10/05/DYQFazEXBG7Uwcn.png)

# 背景分割

通过不同方法拟合各种模型来建立起背景图像。之后再将当前帧与背景图像进行相减，从而得到运动的区域。

假设我们当前已经获得了某个背景图片，此时镜头内会出现障碍物，例如车辆或其他移动物体出现在摄像头内并遮挡背景，这种时候我们就可以使用这种方法来得到我们视野内的有关运动物体的位置。

基于这种思想，在OpenCV库中我们可以使用相关的背景分割器来进行相关的操作。

### 相关算法

1. KNN：基于K最近邻进行背景建模，根据附近颜色的相似度来进行的背景提取。
2. MOG2：基于混合高斯进行背景建模，MOG的升级版本。

### 范例代码

```aardio
import cv2

//示例视频下载地址 https://raw.githubusercontent.com/opencv/opencv/3.1.0/samples/data/768x576.avi
cap = cv2.VideoCapture("./assets/images/768x576.avi")
subtractor = cv2.createBackgroundSubtractorMOG2()
//subtractor = cv2.createBackgroundSubtractorKNN()
k = cv2.getStructuringElement( 1/*_CV2_MORPH_CROSS*/,::SIZE(3,3) )
ok,img = cap.read()
while(ok){
	cv2.imshow("原图",img)
	code = cv2.waitKey(50)
	fmask = subtractor.apply(img)
	ret,binary = cv2.threshold( fmask,220,255,0x0/*_CV2_THRESH_BINARY*/)
	binary = cv2.morphologyEx( binary,2/*_CV2_MORPH_OPEN*/,k)
	cv2.imshow("蒙版图",binary)
	code = cv2.waitKey(50)
	fmask.release()
	binary.release()
	if(code==0x1B/*_VK_ESC*/){
		bk = subtractor.getBackgroundImage()
		cv2.imshow("背景图",bk)
		cv2.waitKey(0)
        break
    }
    err,img = cap.read()
}
```



![](https://i.loli.net/2021/10/05/LZ24ImHRawG6MoQ.png)