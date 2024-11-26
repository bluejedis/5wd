
# "发送"的 全局概览
## example-发送一条推文
```mermaid
graph TD
    A[用户发送推文] --> B[应用层: **HTTP**请求]:::whitetext
    B --> C[传输层: **TCP**分段]:::whitetext
    C --> D[网络层: **IP**路由]:::whitetext
    D --> E[数据链路层: **帧**处理]:::whitetext
    E --> F[物理层: 信号传输]
    F --> G[推特服务器]
classDef whitetext fill:#fff
```
1. 应用层 (Application Layer)
   - 处理特定应用程序的协议
     - HTTP (网页)
     - FTP (文件传输)
     - SMTP (邮件)
     - 游戏专用协议
   - 生成用户数据
2. 传输层 (Transport Layer)
  - 数据分段
  - 添加信息
    - 端口号
    - 序列号
  -协议
    - TCP (可靠传输)
    - UDP (快速传输)

3. 网络层 (Network Layer)
  - IP协议处理
  - 添加地址信息
    - 源IP地址
    - 目标IP地址
  - 路由选择
  - 数据包转发
4. 数据链路层 (Data Link Layer)
   - 数据分帧
   - MAC地址处理
   - 错误检测
   - 流量控制
5. 物理层 (Physical Layer)
- 数据信号转换
  - 数字信号
  - 电信号
  - 光信号
- 物理传输介质
  - 网线
  - 光纤
  - 无线电波
## H1→R1→R2→H2
- ![](https://cdn-mineru.openxlab.org.cn/model-mineru/prod/e7e297fbb0520613c7252ae1f3530980f5248c701458e45dd8079c49662f2683.jpg)  
图3.1主机H1向H2发送数据  

- ![](https://cdn-mineru.openxlab.org.cn/model-mineru/prod/25920ca739682fceb5264e70ee939f8d7abe7feee0fa034448b85f86ab86b5f8.jpg)  
图3.2从层次上看数据的流动  
## 网络层具体的传输过程
- 地址解析流程
- NAT处理流程
- 路由选择流程
- MTU检查和分片
- 帧封装过程
- 
  ```mermaid
  flowchart TB
    Start[IP数据报]:::whitetext --> AddrCheck{需要地址解析?}
    
    AddrCheck -->|是| ARP[ARP协议处理]:::whitetext
    ARP --> ARPCache{ARP缓存?}
    ARPCache -->|有| GetMAC[获取MAC地址]:::whitetext
    ARPCache -->|无| ARPRequest[发送ARP请求]
    ARPRequest --> GetMAC
    
    AddrCheck -->|否| NATCheck{需要NAT?}
    GetMAC --> NATCheck
    
    NATCheck -->|是| NAT[NAT协议处理]:::whitetext
    NAT --> NATTable[更新NAT表]
    NATTable --> Route[路由选择]
    
    NATCheck -->|否| Route
    
    Route --> RouteProtocol[路由协议处理]:::whitetext
    RouteProtocol --> RouteTable[查询路由表]
    RouteTable --> NextHop[确定下一跳]
    
    NextHop --> MTUCheck{大于MTU?}:::whitetext
    MTUCheck -->|是| Fragment[分片处理]
    Fragment --> Frame[封装成帧]:::whitetext
    
    MTUCheck -->|否| Frame
    
    Frame --> AddHeader[添加帧头帧尾]
    AddHeader --> Checksum[计算校验和]
    Checksum --> End[发送到数据链路层]
    classDef whitetext fill:#fff
  ```
## data的变换过程
### **传输层+网络层**:
<div style="display: flex;">
  <img src="https://cdn-mineru.openxlab.org.cn/model-mineru/prod/05fa857d9ae050353446295e04e96bef0c74167c8553e5cf4439dd5c06e12f8d.jpg" alt="UDP数据报" style="width: 550px; margin-right: 10px;">
  <img src="https://cdn-mineru.openxlab.org.cn/model-mineru/prod/4f25e5e7af600a926e06cfbeecdb4d247bc24f97f4e1b6d8267cb4459a094260.jpg" alt="TCP报文段" style="width: 600px;">
</div>

#### IP数据报封装过程
- 数据封装顺序
  1. 应用层**数据** -> 【传输层封装 -> 网络层封装】 -> 数据链路层封装
- 传输层处理
  * TCP将数据封装成TCP报文段
  * UDP将数据封装成UDP数据报
- 网络层处理
  * 接收来自传输层的TCP报文段或UDP数据报
  * 添加IP首部，形成IP数据报
  * IP数据报包含了完整的TCP报文段或UDP数据报

关键说明
* IP数据报是在网络层新增IP首部后形成的
* 不是将TCP/UDP数据报分离后形成的
* **每一层**都是在**上一层**的**基础上** **添加**自己的**首部信息**
* 这是一个自上而下的封装过程

### IP数据报 → 帧

#### 每一层的首部都会向下传递，作为下一层的<span style="color: blue;">数据</span>部分
这是网络协议分层封装的基本原则。

1. 应用层数据
   - 应用层数据向下传递给传输层

2. 传输层封装
   - TCP/UDP首部 + 应用层数据
   - 整体向下传递给网络层

3. 网络层封装
   - IP首部 + (TCP/UDP首部 + 应用层数据)
   - 整体向下传递给数据链路层

4. 数据链路层封装
   - 帧首部 + (IP首部 + TCP/UDP首部 + 应用层数据) + 帧尾部


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
## IP v4——Subnetting
### 将一个C类网络208.115.21.0划分为4个子网
前面的数字保持不变，后面的.0依次替换为：
$00 = 0 $
$01 = 1 * 2^6 = 64 $
$10 = 1 * 2^7 = 128 $
$11 = 1 * 2^6 + 1 * 2^7 = 192 $
即
子网1：208.115.21.0
子网2：208.115.21.64
子网3：208.115.21.128
子网4：208.115.21.192
<span style="font-size: 14px;">一个C类IP地址，其结构是前24位用于网络地址，后8位用于主机地址。
当这8位中的2位用于子网地址时，我们实际上是在使用这8位中的最高两位来表示子网。

#### 三类IP地址& 使用范围
![](https://cdn-mineru.openxlab.org.cn/model-mineru/prod/f2e278ae9a152d96978ffc45243b7b127a36a1cce3578e4c25be1b688bd910d2.jpg)

![](https://cdn-mineru.openxlab.org.cn/model-mineru/prod/8e5f99e160787c8b792c1180313a4d2d4e023d0e54160b7414f055ff18773b94.jpg)
## IPv4——Subnet Mask
- eg：(128.14.32.5/20)
  - 掩码是20个连续的1和后续12个连续的0
  - IP=10000000.00001110.00100000.00000101
  - 掩码=11111111.11111111.11110000.00000000
  - 网络前缀=10000000.00001110.00100000.00000000(128.14.32.0)

二进制运算 2^8=128

## IPv4——ARP ：硬件地址=MAC地址<span style="font-size: 14px;">（**M**edia **A**ccess **C**ontrol Address）
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

### ARP
ARP（Address Resolution Protocol）是IPv4协议族中的一种协议，它允许IPv4设备在局域网内通过MAC地址和IP地址进行通信。
#### 4种情况：
直接(连接的网络/路由器) → 直接用ARP在表中查找 MAC地址
间接（非直接连接的网络 上的主机/路由器）→ 用ARP找到下一个路由的MAC地址，剩下的交给它

# Q
- IPv4
- IPv6
