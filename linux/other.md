# 一些杂项
双系统下，windows的快速开机方式其实是进行了深度睡眠，这样windows关机-开机进入ubuntu会使得windows的硬盘空间变为只读。
解决方法：
1. windows下重启代替关机，再进入ubuntu
2. ```sudo ntfsfix /dev/nvme1n1p4```
