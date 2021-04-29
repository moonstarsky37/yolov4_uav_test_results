# yolov4_UAV_test_results
Test results a Unmanned Aerial Vehicles (UAV) image dataset with yolov4 object detection model. Convert orijinal images to video by FFmpeg.

![moonstarsky37/yolov4_uav_test_results/master/results_01.png](https://raw.githubusercontent.com/moonstarsky37/yolov4_uav_test_results/master/results_01.png =300x300)
## Dataset
I using the images in this datasets and convert images to video for inference.
* site: https://sites.google.com/view/grli-uavdt/
* citation:
• D. Du, Y. Qi, H.g Yu, Y. Yang, K. Duan, G. Li, W.g Zhang, Q. Huang, Q. Tian, " The Unmanned Aerial Vehicle Benchmark: Object Detection and Tracking", European Conference on Computer Vision (ECCV), 2018. 

## Yolov4 Object Detection Model
Install darknet yolov4 in this repository https://github.com/AlexeyAB/darknet.
Note that we will set the **darknet** as environment parameters.


## FFMPEG
If your OS is linux or macOS, you can convert list of images in folder with folling command like:
```bash=
ffmpeg -framerate 1 -pattern_type glob -i '*.png' \
  -c:v libx264 -r 30 -pix_fmt yuv420p out.mp4
```
The **glob** feature cannot using in Windows till now. If you want to do the same thing, you can rename the images with pattern similar to this (ref)[https://stackoverflow.com/questions/24961127/how-to-create-a-video-from-images-with-ffmpeg] in stackoverflow.
```bash=
file 'E:\images\png\images__%3d.jpg'
file 'E:\images\jpg\images__%3d.jpg'
```

Alternatively, I create a file named **folderImg2video.cmd** with following 


```bash=
@echo off
setlocal enabledelayedexpansion
set arg1=%1
:echo %arg1%
for /f %%A in ('dir /s /b "./%%arg1%%"') do (
	set line=%%~fA
	set newline=!line:\=/!
	echo file !newline! >>tmp.txt
)
ffmpeg -r 24 -safe 0 -f concat -i tmp.txt -c:v libx264 -pix_fmt yuvj420p %arg1%.mp4
del tmp.txt
```
And just execute this sctipts by 
```
$ folderImg2video.cmd <your_image_folder_path>
```

## Prediction by yolov4
Just execute
```
$ darknet detector demo cfg/coco.data cfg/yolov4.cfg weights/yolov4.weights -ext_output ./test_M02.mp4
```
