# Docker相关
## 配置
### 镜像加速
```
sudo vim /etc/docker/daemon.json
```
这里加入了nvidia-docker的daemon,再加入镜像下载加速，则修改为：
```
{
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
    },
     "registry-mirrors": ["https://cbrok4rc.mirror.aliyuncs.com"]
}
```
重启docker服务
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```
## 容器与镜像管理
* 查看容器与镜像
```
docker ps -a
docker images
```
* 删除容器与镜像
```
docker rm [container_name/container_id]
docker rmi [image_id]
```
* 新建容器 
```
nvidia-docker run -it --ipc=host -v /home/elpeczmk6/Project/PIXOR:/pixor --name='pixor' be795ae7f606
```
nvidia-docker 也可以换为 --gpus all
-it 容器属性设置（待补充)  
--ipc=host 设置容器的IPC属性为宿主机  *或者* --shm-size=1G 设置共享内存，这点尤其需要在pytorch环境下设置，因为pyorch使用共享内存来进行进程之间的数据交互，镜像默认的64MBshm并不够用。  
-v 建立宿主机和容器内部文件夹的映射 be79..是镜像ID  
新建容器应当使用nvidia-docker run 只用docker不分配显卡 
* commit镜像  
```
docker commit -a 'Elpecz' -m 'a image of pixor' pixor pixor:torch1_tf1.12_cu9_cv2
```
-a 作者名 -m 描述 名字：tag

* 导出导入镜像   
```
docker save yolov4 -o ./yolov4.tar
或者
docker save yolov4:cu10.2_cv -o ./yolov4.tar
使用name:tag的方式可以保留name:tag到.tar的镜像，如果只是用name则会丢失name tag为 none

docker load -i ./yolov4.tar
``` 
* 重命名镜像
可以使用命令:  
```
docker tag [image id] [name]:[版本]
```
例如:
```
docker tag b03b74b01d97 iecas8:cv2_cu10.2_torch1.0
```
## 新建docker容器后要做的事情：  
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



## docker 及nvidia docker安装：  
有官方的最好参考官方指导  

储存空间管理  
https://www.cnblogs.com/jakaBlog/p/10538382.html
```
docker info
```
