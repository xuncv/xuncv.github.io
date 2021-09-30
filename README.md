# 简介

**opencv_aardio**是使用aardio封装的OpenCv开源计算机视觉库.接口实现上尽量接近[opencv-python](https://pypi.org/project/opencv-python/)风格,以降低学习成本.本库适配最新OpenCv 4.5.3

#### 依赖项目:

1. [aardio](http://www.aardio.com/)
2. [OpenCv](https://opencv.org/)
3. [OpenCvSharp](https://github.com/shimat/opencvsharp)
4. [opencv-plugin](https://github.com/xuncv/opencv-plugin/) (修改自[OpenCvSharp](https://github.com/shimat/opencvsharp))

```aardio
import cv2
img = cv2.imread("./images/Lena.jpg",1)
img = cv2.medianBlur(img,5)
cv2.imshow( "窗口标题",img )
cv2.imwrite("result.jpg",img)
cv2.waitKey(0)
```

![](https://i.loli.net/2021/09/30/VcB9TFawIusfzvE.png)



### YOLO

![img](https://github.com/xuncv/opencv_aardio/raw/main/images/detect.jpg)
