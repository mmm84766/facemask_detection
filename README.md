# face-mask-detector
𝐑𝐞𝐚𝐥-𝐓𝐢𝐦𝐞 𝐅𝐚𝐜𝐞 𝐦𝐚𝐬𝐤 𝐝𝐞𝐭𝐞𝐜𝐭𝐢𝐨𝐧 


## System Overview

It detects human faces with 𝐦𝐚𝐬𝐤 𝐨𝐫 𝐧𝐨-𝐦𝐚𝐬𝐤 even in crowd in real time with live count.

**System Modules:**
  
1. **Deep Learning Model :** I trained a YOLOv4 on my own dataset and for YOLOv4 achieved **93.95% mAP on Test Set** whereaseven though my test set contained realistic blur images, small + medium + large faces which represent the real world images of average quality.  
  



## Quick-Start
**Step 1:**
```
git clone https://github.com/adityap27/face-mask-detector.git
```
Then, Download weights. https://bit.ly/yolov4_mask_weights and put in **yolov4-mask-detector** folder

**Step 2: Install requirements.**
```
pip install opencv-python
pip install imutils
```
**Step 3: Run yolov4 on webcam**
```
python mask-detector-video.py -y yolov4-mask-detector -u 1
```



### 3. Dataset Description
- Dataset is split into 3 sets:

|_Set_|Number of images|Objects with mask|Objects without mask|
|:--:|:--:|:--:|:--:|
|**Training Set**| 700 | 3047 | 868 |
|**Validation Set**| 100 | 278 | 49 |
|**Test Set**| 120 | 503 | 156 |
|**Total**|920|3828|1073|



## Deep Learning Models

### 1. Training
- Install [Darknet](https://github.com/AlexeyAB/darknet) for Mac or Windows first.
- I have trained YOLOv4.
- Use following (linux) cmd to train:


```console
./darknet detector train obj.data yolo3.cfg darknet53.conv.74
```



**YOLOv4 Training details**

- Data File = [obj.data](https://raw.githubusercontent.com/adityap27/face-mask-detector/master/yolov4-mask-detector/obj.data)
- Cfg file  = [yolov4-obj.cfg](https://raw.githubusercontent.com/adityap27/face-mask-detector/master/yolov4-mask-detector/yolov4-obj.cfg)
- Pretrained Weights for initialization= [yolov4.conv.137](https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.conv.137)
- Main Configs from yolov4-obj.cfg:
	- learning_rate=0.001
	- batch=64
	- subdivisions=64
	- steps=4800,5400
	- max_batches = 6000
	- i.e approx epochs = (6000*64)/700 = 548
- **YOLOv4 Training results: _1.19 avg loss_**
- **Weights** of YOLOv4 trained on Face-mask Dataset: [yolov4_face_mask.weights](https://bit.ly/yolov4_mask_weights)

### 2. Model Performance
- Below is the comparison on YOLOv4 on 3 sets.
- **Metric is mAP@0.5** i.e Mean Average Precision.
- **Frames per Second (FPS)** was measured on **Google Colab GPU - Tesla P100-PCIE** using **Darknet** command: [link](https://github.com/AlexeyAB/darknet#how-to-evaluate-fps-of-yolov4-on-gpu)


### 3. Inference

- You can run model inference or detection on image/video/webcam.
- Two ways:
	1. Using Darknet itself
	2. Using Inference script (detection)

### 3.1 Detection on Image
- Use command:
	```
	./darknet detector test obj.data yolov4.cfg yolov4_face_mask.weights input/1.jpg -thresh 0.45
	```
	OR
- Use inference script
	```
	python mask-detector-image.py -y yolov4-mask-detector -i input/1.jpg
	```
- **Output Image:**
	
	![1_output.jpg](https://github.com/mmm84766/facemask_detector/blob/main/output/1_output.jpg?raw=true)


### 3.2 Detection on Video
- Use command:
	```
	./darknet detector demo obj.data yolov4.cfg yolov4_face_mask.weights <video-file> -thresh 0.45
	```
	OR
- Use inference script
	```
	python mask-detector-video.py -y yolov4-mask-detector -i input/task_video.mp4 -u 1
	```
	

	



