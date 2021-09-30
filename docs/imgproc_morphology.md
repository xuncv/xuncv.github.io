# 图像形态学

### 形态学操作

形态学操作是指改变图像中图形的形状,一般作用于二值化图像中.二值化图像只有`0` `255`二个值,确切的讲,形态学操作改变的是255值(白色)的图形形状,使之在设定的核形状上"变胖"或"变瘦".

### 1.结构元素

结构元素也叫做核,形态学操作即应用结构元素做卷积.结构元素可以是矩形/椭圆/十字形,可以用``cv2.getStructuringElement`来生成不同形状的结构元素

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

