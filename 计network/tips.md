# IP
IP在TCP/IP中的位置:
![image](https://bluejedis.github.io/picx-images-hosting/image.3yegz5qm1p.webp)

## IPv4首部：
![image](https://bluejedis.github.io/picx-images-hosting/image.syz0a16dr.webp)
H首部长度：4位，基本单位是4字节。

T总长度：16位，基本单位是1字节。

F片偏移：13位，基本单位是8字节。

### 占位 与 基本单位
基本单位
- 字段在计算长度时的单位
- 为了方便计算和处理而设定
- not直接表示字段在首部中的位数

占位
- 字段在首部中的位数

两者没有任何的connection

### Fragmentation 计算
已知
要求：

# 硬件地址=MAC地址<span style="font-size: 14px;">（**M**edia **A**ccess **C**ontrol Address）
硬件地址（Hardware Address），也称为物理地址（Physical Address）或**MAC地址**（Media Access Control Address），是分配给网络接口控制器（NIC）的唯一标识符。
用于局域网（LAN）或其他网络技术中进行通信。MAC地址通常由48位或64位二进制数组成，以十六进制表示，并且全球唯一。

MAC地址的作用是在网络中**标识**和区分不同的**设备**，使得数据包能够在网络中准确地从一个设备传输到另一个设备。
- 位于OSI模型的第二层，即数据链路层
- <span style="font-size: 14px;">IP地址，位于第三层，即网络层

MAC地址的格式通常表示为六个字节，每个字节由两个十六进制数字组成，字节之间用冒号或短横线分隔。例如，00:1A:2C:3D:4E:5F就是一个MAC地址的例子。

<span style="color: gray;font-size: 14px;"> Windows系统中
<figure>
&emsp;&emsp;<span style="font-size: 14px;">可以通过命令行使用ipconfig /all命令来查看电脑的MAC地址，或者通过系统网络中心来查看。

&emsp;&emsp;<span style="font-size: 14px;">可以在连接到Wi-Fi时使用随机硬件地址，以增强隐私保护。


