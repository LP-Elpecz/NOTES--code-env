# Docker相关
新建容器 
```
nvidia-docker run -it -v /home/elpeczmk6/Project/PIXOR:/pixor --name='pixor' be795ae7f606
```
-it 容器属性设置（待补充） -v 建立宿主机和容器内部文件夹的映射 be79..是镜像ID
commit镜像  
```
docker commit -a 'Elpecz' -m 'a image of pixor' pixor pixor:torch1_tf1.12_cu9_cv2
```
-a 作者名 -m 描述 名字：tag

导出导入镜像   
```
docker save yolov4 -o ./yolov4.tar
docker load -i ./yolov4.tar
``` 
新建docker应当使用nvidia-docker run 只用docker不分配显卡  
新建docker容器后要做的事情：  
1. 验证nvidia及cuda， torch，tf  
```
nvidia-smi
nvcc -V  

python
import torch
import tensorflow
```
2. apt换源  
https://blog.csdn.net/qq_21095573/article/details/99736630  
3. pip及conda换源  

Docker 安装，及储存空间管理  
https://www.cnblogs.com/jakaBlog/p/10538382.html
```
docker info
```

docker 及nvidia docker安装：  
有官方的最好参考官方指导  
