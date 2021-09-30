# 图像输入和转码

### 1. 打开 显示 保存图片

 ```aardio
 import cv2
 img = cv2.imread("./images/Lena.jpg",1/*_CV2_IMREAD_COLOR*/)
 cv2.imshow( "窗口标题",img )
 cv2.imwrite("./images/result.jpg",img,{1/*_CV2_IMWRITE_JPEG_QUALITY*/,80})
 ```

### 2. 从字节流中加载图片

```aardio
import inet.http
import cv2
buffer = inet.http().get("https://bbs.aardio.com/static/image/common/aardio.png")
img = cv2.imdecode(buffer,1/*_CV2_IMREAD_COLOR*/)
cv2.imshow("logo",img)
cv2.waitKey(0)
```

### 3. 图片转字节流

```aardio
import inet.http
import cv2
buffer = inet.http().get("https://bbs.aardio.com/static/image/common/aardio.png")
img = cv2.imdecode(buffer,1/*_CV2_IMREAD_COLOR*/)
//顺便从png转jpg格式
bin = cv2.imencode(".jpg",img)
string.save("./result.jpg",bin )
```

