# mat类



| opencv类型结构（C++常量） | aardio格式 | numpy格式 | 注释 |
| ------------------------- | ---------- | --------- | ---- |
|                           |            |           |      |
|                           |            |           |      |
|                           |            |           |      |
|                           |            |           |      |
|                           |            |           |      |
|                           |            |           |      |
|                           |            |           |      |
|                           |            |           |      |
|                           |            |           |      |

## 矩阵操作

### 多通道阈值二值化 cv2.inRange

将介于[lowerb(第二参数), upperb(第三参数)]间的像素值变成255，其他变为0。

```aardio
import cv2
src = cv2.imread("./assets/images/flower.png")
hsv = cv2.cvtColor(src,0x28/*_CV2_COLOR_BGR2HSV*/)
mask = cv2.inRange(hsv,::Scalar(156,43,46),::Scalar(180,255,255))
cv2.imshow("mask", mask)

result = cv2.bitwise_and(src,src,mask)
cv2.imshow("result", result)
cv2.waitKey(0)
```

![](https://i.loli.net/2021/10/06/zn9CxVAh7UDb6jJ.png)



### 图像比较 cv2.compare

计算两个图像之间的差异。

>两张图需有相同的尺寸和通道数

```aardio
import cv2
src1 = cv2.imread("./assets/images/Lena.jpg")

src2 =src1.clone()
//一张图写入文字
cv2.putText(src1,"Lena",::POINT(20,20))
dst = cv2.compare(src1,src2,0/*_CV2_CMP_EQ*/)
cv2.imshow("src1",src1)
cv2.imshow("src2",src2)
cv2.imshow("dst",dst)
cv2.waitKey(0)
```

![](https://i.loli.net/2021/10/06/5x4uvwpDZIBVYoK.png)
