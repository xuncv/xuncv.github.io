# samples

## 1. 玉米粒计数



```aardio

```

## 2. 口罩检测

```aardio
import cv2

class faceMask{
	ctor( confThresh=0.5,iouThresh=0.4  ){
		//..cv2.setNumThreads(4)
		this.featureMapSizes = {{33, 33}, {17, 17}, {9, 9}, {5, 5}, {3, 3}};
		this.anchorSizes = {{0.04, 0.056}, {0.08, 0.11}, {0.16, 0.22}, {0.32, 0.45}, {0.64, 0.72}};
		this.anchorRatios =  {1, 0.62, 0.42};
		this.variances = {0.1, 0.1, 0.2, 0.2};
		this.targetShape = ::SIZE(260,260)
		this.numPrior = 5972
		this.priorData = {}//..table.array(this.numPrior*4,0)
		this.net = ..cv2.dnn.readNet("./assets/model/face_mask_detection.caffemodel","./assets/model/face_mask_detection.prototxt")
		this.net.setConfig({
			confThreshold = confThresh; 
			nmsThreshold = iouThresh; 
		})
		this.outLayersNames = this.net.getUnconnectedOutLayersNames()
	};
/*gen{{*/
	generatePriors = function(){
		owner.priorData = {}
		var height = 0;
		var width = 0;
		var ratio = 0;
		//..table.array()
		for(i=1;5;1){
			featureMapHeight = this.featureMapSizes[i][1];
			featureMapWidth = this.featureMapSizes[i][2];
			for(h=1;featureMapHeight;1){
				for(w=1;featureMapWidth;1){
					ratio = ..math.sqrt(this.anchorRatios[1])
					for(j=1;2;1){
						width = this.anchorSizes[i][j] * ratio
						height = this.anchorSizes[i][j] / ratio
						..table.push(this.priorData,
									(w-1+0.5)/featureMapWidth,
									(h-1+0.5)/featureMapHeight,
									width,height
									)
					}
					for(j=1;2;1){
						ratio = ..math.sqrt(this.anchorRatios[j+1])
						width = this.anchorSizes[i][1] * ratio
						height = this.anchorSizes[i][2] / ratio
						..table.push( this.priorData,
									  (w-1+0.5)/featureMapWidth,
									  (h-1+0.5)/featureMapHeight,
									   width,height							
						)
					}
				}
			}
		}
	}
	/*}}*/    
  
	decode = function(outs){
		this.generatePriors()
		var loc = outs[1]
		var conf = outs[2]
		var boxes = {}
		var confidences = {}
		var classIds = {}
    	var floatSize = ..raw.sizeof({float v})
    	if(loc.dims==3){
    		loc = loc.reshape(0,this.numPrior)
    	}
    	if(conf.dims==3){
    		conf = conf.reshape(0,this.numPrior)
    	}
    	var locData = loc.data
    	for(i=1;this.numPrior;1){
    		scores = conf.row(i-1).colRange(0,2)
    		var minVal,maxVal,minLoc0,maxLoc0 = ..cv2.minMaxLoc(scores)
    		if(maxVal>this.net.$config.confThreshold){
    			var rowInd = (i-1)*4
    			var floatArray = ..raw.convert(locData,{float value[4]}, rowInd*4).value
    			predictW = ..math.exp(floatArray[3] * this.variances[3]) * this.priorData[rowInd + 3]
    			predictH = ..math.exp(floatArray[4] * this.variances[4]) * this.priorData[rowInd + 4]
    			predictXmin = floatArray[1] * this.variances[1] * this.priorData[rowInd+3] + this.priorData[rowInd+1] - 0.5 * predictW
    			predictYmin = floatArray[2] * this.variances[2] * this.priorData[rowInd+4] + this.priorData[rowInd+2] - 0.5 * predictH
    			..table.push( classIds,maxLoc0.x)
    			..table.push( confidences, maxVal)
    			..table.push( boxes,::RECT(predictXmin*this.net.size.cx,predictYmin*this.net.size.cy,predictW*this.net.size.cx,predictH*this.net.size.cy) )
    		}
    	}
    	return boxes,confidences,classIds; 
	}

	detect = function(img){
		var blob = this.net.blobFromImage(img,1/255,this.targetShape)
		this.net.setInput(blob)
		var outs = this.net.forward(this.outLayersNames)
		var boxes,confidences,classIds = this.decode(outs)
		var indices = ..cv2.dnn.nmsBoxes(boxes,confidences,this.net.$config.confThreshold,this.net.$config.nmsThreshold)
		
		var filterBoxes = {}
		var filterScores = {}
		var filterIds = {}

		for(i=1;#indices;1){
			var idx = indices[i];
			var rect = boxes[idx+1]
			..table.push( filterBoxes,rect )
			..table.push( filterScores,confidences[idx+1] )
			..table.push( filterIds,classIds[idx+1]+1)
		}

		//还原回0-1的浮点矩阵
		filterBoxes = ..table.map(filterBoxes,function(v,k,result){
			return ::RECTF(	v.left/this.targetShape.cx,
							v.top/this.targetShape.cy,
							v.right/this.targetShape.cx,
							v.bottom/this.targetShape.cy
							); 
		})
		return filterBoxes,filterScores,filterIds; 
	}
}

//推理调用
faceMaskDetector = faceMask()

var tk = time.tick()
src = cv2.imread("./assets/images/img5.jpeg")
presrc = src.clone()
src = cv2.cvtColor(src,4/*_CV2_COLOR_BGR2RGB*/)
boxes,scores,classIds = faceMaskDetector.detect(src)
costTime = time.tick() - tk
var h,w,c = table.unpack( src.shape )
table.map(boxes,function(v,k,result){
    var x = v.x*w
    var y = v.y*h
    var x2 = (v.x+v.width)*w
    var y2 = (v.y+v.height)*h
	cv2.rectangle(presrc,::POINT(x,y),::POINT(x2,y2),::Scalar(0,255,0),3)

	var text = string.format("%s %.4f",classIds[classIds[k]]==1 ? "no mask" : "wear mask",scores[k] )
	var textRectSize = cv2.getTextSize(text,0/*_CV2_FONT_HERSHEY_SIMPLEX*/,0.5,1)
	cv2.rectangle(presrc,::POINT(x,y),::POINT(x+textRectSize.cx,y+textRectSize.cy+15),::Scalar(0,255,0),-1)
	cv2.putText( presrc,text,::POINT(x,y+15),,0.5,::Scalar(255,0,0) )
})

cv2.imshow(string.format("耗时:%dms",costTime),presrc)
cv2.waitKey(0)

//参考项目 //https://github.com/AIZOOTech/FaceMaskDetection
```

![](https://i.loli.net/2021/10/06/7cDsv56QJVy8toC.png)
