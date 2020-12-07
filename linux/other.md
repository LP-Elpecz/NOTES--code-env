# 一些杂项
## 解决windows-ubuntu双系统下，ubuntu系统无法写入windows系统盘

双系统下，windows的快速开机方式其实是进行了深度睡眠，这样windows关机-开机进入ubuntu会使得windows的硬盘空间变为只读。  
解决方法：  
·  windows下重启代替关机，再进入ubuntu  
·  关闭windows的快速启动  
cmd下：  
```
powercfg /h off
```

### 坑
***该条目意味着你可以在现有的论坛上看到下面的解决方法，但是方法是错误的。起码在我的实践中被证实无效甚至出现bug和崩溃，请谨慎使用或者干脆不使用***  
· 使用该方法可以在ubuntu下修复windows盘，但是破坏了windows的文件管理逻辑，导致你在ubuntu下对windows盘的写入和修改操作无效。  
 ```sudo ntfsfix /dev/nvme1n1p4```
