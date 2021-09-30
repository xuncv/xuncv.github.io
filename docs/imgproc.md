# 图像处理

### 1. 调整图像大小 cv2.resize

函数原型:cv2.resize(源数据,::SIZE对象,x轴缩放倍数,y轴缩放倍数,插值方式) 

> 插值方式常量 _CV2_INTER

| 常量               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| _CV2_INTER_NEAREST | 最近邻插值                                                   |
| _CV2_INTER_LINEAR  | 线性插值                                                     |
| _CV2_INTER_CUBIC   | 双线性插值                                                   |
| _CV2_INTER_AREA    | 使用像素区域关系重新采样。它可能是图像抽取的首选方法，因为它可以提供无莫尔条纹的结果。但是当图像被缩放时，它类似于INTER_NEAREST方法。 |



### 2. 转换颜色空间 cv2.cvtColor

opencv默认颜色空间为`BGR`,支持的颜色空间有 `RGB`、`HSI`、`HSL`、`HSV`、`HSB`、`YCrCb`、`CIE XYZ`、`CIE Lab` 8种

函数原型:cv2.cvtColor(输入图像,颜色空间代码) 

> 颜色空间代码常量 _CV2_COLOR_

