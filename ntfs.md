# NTFS 文件流（ADS）隐藏 

## 具体操作：　　
cmd命令：type 1.jpg>>2.txt:1.jpg

## 注意事项：
> 1. 流文件不能直接通过网络传输，必须使用WinRAR在压缩时勾上高级选项中“保存文件流数据”。
> 1. 制作好的流文件大小跟原文件是一样的，只在压缩后才包含隐藏文件的大小，说明NTFS文件流仍然会占用磁盘空间。
> 1. 流文件只能在NTFS分区储存和运行(压缩“保存文件流数据”也可储存到FAT32，但解压出来的文件依然是丢失了数据流的文件)，一旦放到其它的文件系统中，即使再放回来，NTFS数据流也会丢失。

### 所以隐藏的流究竟是什么？
### 我们要怎么发现隐藏的流呢？

答案是交换数据流(alternate data stream)

> NTFS使用主文件表(Master File Table)来管理文件，系统中每个文件都会有一个MFT记录与之对应。而每一个MFT记录都由若干用来描述这个记录的属性组成。其中$DATA属性就记录了文件的具体内容。用操作系统的话讲，就是索引节点和数据区块的关系。

> 而一个文件的$DATA属性并不是唯一的。当然，每个文件都有一个主数据流(PDS)，也就是所有人都能看到的外显数据。这是一个文件创建时就默认创建的数据，查看文件时也只会显示主数据流。数据的隐藏，则发生在我们后创建的交换数据流中(ADS)。

> 比如“type 1.jpg>>2.txt:1.jpg”这一条命令，就是将1.jpg作为了文件2.txt的交换数据流，所以查看文件（主数据流）的操作不能发现1.jpg。

我们在侦测这个流的时候可以通过cmd和powershell的命令


（当然网上也有流检测的工具，不过没有想象中的好用


> 发现流：dir /r
![image](https://github.com/xxy961216/xxy961216.github.io/blob/master/1.png)
> 获取流：Get-Content 主文件名 –stream 交换数据流名
![image](https://github.com/xxy961216/xxy961216.github.io/blob/master/2.png)
