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