# day5-RT-thread 软件包学习（文件系统）

## 1.学习内容

今天主要讲了关于rtt软件包的使用，导师列举了rtt文件系统，以及阿里云mqtt，还有aht20软件包的使用。我来简单的复现一下

### rtt文件系统
[rtt官方文件系统介绍](https://www.rt-thread.org/document/site/#/rt-thread-version/rt-thread-standard/programming-manual/filesystem/filesystem)

首先在env中使能文件系统
![使能文件系统.png](../使能文件系统.png)
![使能文件系统2.png](../使能文件系统2.png)
![开启elm.png](../开启elm.png)
![开启elm2.png](../开启elm2.png)
![更改扇区大小.png](../更改扇区大小.png)

配置完成关闭env编译烧录
![烧录后.png](../烧录后.png)

剩下的操作就跟linux里的指令大差不差了，例如创建文件夹用`mkdir`等命令
![创建文件.png](../创建文件.png)

更新[【2024-RSOC】day5 RTT软件包学习](https://club.rt-thread.org/ask/article/08339a766ece01b1.html)
