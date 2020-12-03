# YOLOV4  
https://github.com/dm0mb/darknet
环境: ubuntu18.04, c++, docker 19.03 + nvidia-docker2  
## 配置：
参考：https://blog.csdn.net/weixin_39617145/article/details/107671077  
其中主要是opencv比较难装，也不是难装，官方给的指引比较模糊。论坛上的经验已经被复制来复制去变得面目全非  
### 安装：opencv3.4.12
按照一般指引安装opencv依赖：  
```
 sudo apt-get install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg-dev libswscale-dev libtiff5-dev
 sudo apt-get install libgtk2.0-dev
 sudo apt-get install pkg-config
```
下载源码之后，解压，cd 进入文件夹  
```
 mkdir build
 cd build
 cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_GTK=ON -D OPENCV_GENERATE_PKGCONFIG=YES ..
```
关键在于 GENERATE_PKGCONFIG=YES 否则缺失对依赖文件的路径设置  
如果需要使用opencv-contrib, 加入如下代码到上面的命令中:  
*-D OPENCV_EXTRA_MODULES_PATH=/yolov4/opencv-3.4.12/opencv_contrib/modules/*  
之后make, install
```
 make -j8
```
(线程数你电脑抗得住就行)
```
 make install
```

添加路径，打开ld.so.conf
```
 vi etc/ld.so.conf
```
加入一行
include usr/local/lib  

打开bash.bashrc
```
 vi /etc/bash.bashrc
```
加入两行  
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig  
export PKG_CONFIG_PATH  
```
source /etc/bash.bashrc
```
最后检验：
```
 pkg-config opencv --modversion
```
## YOLOv4运行
### 检测单张图片
```
 ./darknet detect cfg/yolov4.cfg yolov4.weights data/dog.jpg
```
### 训练
```
 ./darknet detector train data/obj.data yolo-obj.cfg yolov4.conv.137
```

可选参数  
**-dont_show -gpus 0,1,2,3**   **-show_img** 展示训练图片 **-Loss -map** 记录loss和val’s map为散点图  
在./data文件夹下建立 obj.names (每一行是一个类别的名称)以及obj.data:  
>classes = 2  
>train  = data/train.txt  
>valid  = data/test.txt 
>names = data/obj.names  
>backup = backup/  

在./data下放置train.txt test.txt,格式如下：

>data/obj/img1.jpg  
>data/obj/img2.jpg  
>data/obj/img3.jpg  

将数据集（.jpg only？）和标注文件（.txt）放入./data/obj中，  
训练命令中 conv.137是预训练参数：https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.conv.137， 预训练网络放在根目录下  
继续训练  
```
 ./darknet detector train data/obj.data yolo-obj.cfg backup\yolo-obj_2000.weights
```
### 测试
```
 ./darknet detector test data/obj.data yolo-obj.cfg yolo-obj_8000.weights
```
### 参数设置技巧：  
* 图像长宽必须是32的整数倍  
* 如果出现OOM，提高subdivisions 到32，64  
* max_batches = 2000*class but >6000 >imges’ number  
* steps = 80%,90% of max_batches  
* 在610，696，783行三个地方修改类别数classes(行号可能有偏差，本文参考官方README，下同)  
* 在603，683，776行三个地方修改filters = (classes + 5)x3   
* learning_rate = 0.00261 / GPUs  
* burn_in = GPUs*1000
### tips:  
* 标注数据命名对应：1.jpg→1.txt 放在同一目录下  
* 数据的命名不能有多个扩展名 如1.jpg.3-1.jpg 只能是 1-3-1.jpg    
* 使用docker镜像save load到另一个环境/机器之后，进入darknet文件夹重新配置yolov4:  
```
./build.sh
```
