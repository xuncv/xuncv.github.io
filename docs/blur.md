# 图像滤波

### 什么是图像滤波
图像滤波，即在尽量保留图像细节特征的条件下对目标图像的噪声进行抑制，是图像预处理中不可缺少的操作，其处理效果的好坏将直接影响到后续图像处理和分析的有效性和可靠性。（摘自网络）

### 图像滤波的目的
1. 消除图像中混入的噪声
2. 为图像识别抽取出图像特征

### 图像滤波的要求
1. 不能损坏图像轮廓及边缘
2. 图像视觉效果应当更好

### 滤波器的种类
3种线性滤波：方框滤波、均值滤波、高斯滤波
2种非线性滤波：中值滤波、双边滤波

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