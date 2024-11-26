<span style="font-size: 45px;">  👻</span>

# TCP 
## 报文
### 端口
  - <span style="color: purple;">不同</span>计算机的 相同<span style="color: blue;">端口</span>号 <span style="color: green;">没有联系
### <span style="color: deepskyblue;">首部</span>
  - 窗口字段的值 → 自己接收窗口的尺寸 
### <span style="color: orange;">报文</span>段
- 对<span style="color: orange;">报文</span>段的确认机制
  -  TCP是面向字节的，对每个字节都进行编号
  -  并不是对接收到的每个字节都要发回确认
     - 收到整个报文段后 → 一个ack
## 连接
### <span style="color: green;">ACK</span>、<span style="color: orange;">SYN</span>、**FIN**=**1**
### 三次handshake
- eg;
  - 设A、B双方发送报文的初始序列号分别为X和Y
    - A发送(①)的报文给B,B接收到报文后发送(②)的报文给A
      -  B 接收到报文后，
         -  发给 A 的确认报文段中应使 
            -  SYN = 1, 使 ACK = 1，且确认号ack = X + 1
            -  即 $\color{green}{ACK}\color{purple}_{X+1}\color{black} = 1$（ACK 的下标为携带的序号），同时告诉自已选择的序号 $\color{blue}seq\color{black} = Y$
      - (ACK的下标为捎带的序号)
      - A发送一个确认报文给B便建立了连接
```mermaid
sequenceDiagram
    participant A
    participant B
    A->>B: SYN=1, seq=X
    B->>A: SYN=1, seq=Y, ACK=1, ACKx+1=1[ack=X+1]
    A->>B: 确认报文(ACK=1, ACKy+1=1[ack=Y+1])
    Note over A,B: TCP连接建立
```
- standard
  - <span style="color: orange;">SYN</span>=1,&emsp;&emsp;&emsp; seg=x 
  - &emsp;&emsp;~ , <span style="color: green;">ACK</span>=1,seq=y, ack=x+1 
  - &emsp;&emsp;×,&emsp;~&emsp;&emsp; ,seq=x+1,ack=y+1 
- 第三个报文
  - as above, 对$ ( ( 建立连接的\color{orange}请求\color{black}^① ) 的\color{green}确认\color{black}^② )的\color{green}确认^③$
### 标志位(<span style="color: green;">ACK</span>、<span style="color: orange;">SYN</span>、**FIN**=**1**)&序号字段(seq/ack)
- explanation
  - 标志位(ACK/SYN/FIN)像开关，只有开(1)和关(0)
  - 序号和确认号像数值，可以是具体的数字

- eg:
  - A → B  seq=200, ack=201,数据部分有2个字节
    - 则B:
      - seq值应和A发向 B的报文中的 ack 值相同，即201
      - ack 值
        - 表示B期望下次收到A发出的报文段的第一个字节的编号(以sqe为standard)
        - 应是200+2=202
    - tips:
      - <span style="color: blue;">seq</span>_B 同<span style="color: green;">ack</span>_A;
      - <span style="color: green;">ack</span>_B以<span style="color: blue;">sqe</span>_A为起始
    - details
      - seq 表示发送的报文段中
        - 数据部分的第一个字节在 $A$ 的发送缓存区中的编号
      - ack 表示A期望收到的
        - 下一个报文段的数据部分的第一个字节在 $B$ 的发送缓存区中的编号
#### <span style="color: green;">ack
- point接收方 希望<span style="color: purple;">next</span>收到的报文段的数据部分 <span style="color: blue;">第一个</span>字节 的编号
  - → get确认号为<span style="color: deepskyblue;">100</span>的确认报文段
    - <span style="color: gray;">→ 接收方希望下一个收到的报文段的数据部分的第一个字节编号为</span>100
    - <span style="color: purple;">末</span>字节序号为<span style="color: green;">99</span>的报文段 已收到
## <span style="color: green;">释放
- 进程中的<span style="color: purple;">any</span>一个都能提出释放连接的请求
## 控制

### <span style="color: green;">r</span>wnd
- rwnd &emsp;&emsp;cwnd  &emsp;&emsp;ssthresh
- 接收窗口  拥塞窗口   慢开始门限
  - <span style="color: green;">r</span>wnd 即**接收方** 允许**连续接收**的**能力**
### <span style="color: deepskyblue;">s</span>wnd 
- <span style="color:deepskyblue;">s</span>wnd 的值 → 可发送窗口的尺寸
  - swnd值由1000变为2000 → 可send 2000
    - (no matter 确不确认)
- 依据：swnd=min[rwnd，cwnd]

### <span style="color: orange;">c</span>wnd
- <span style="color: deepskyblue;">Send</span>端 根据网络拥塞情况确定的窗口值
- 大小
  - 在开始时可以按指数规律增长
### <span style="color: deepskyblue;">滑动</span>wnd
- ~是一种实现<span style="color: green;">流量</span>控制的方法
  - 不是拥塞控制的方法
    - 其机制中用到了滑动窗口
      - 这并不是~ 的作用
- <span style="color: purple;">重传</span>分组的数量
  - (at most) =<span style="color: deepskyblue;">滑动</span>wnd size
- 值设置太大
  - 数据过多→路由器拥挤
  - 主机可能丢失分组
## TCP与UDP
### <span style="color: deepskyblue;">首部
- TCP和UDP首部均含检验和
  - TCP检验和不仅检验数据，还检验TCP首部
  - UDP检验和仅检验数据
- TCP首部独有的是 **seq** 和 **ack**
#### <span style="color: purple;">伪<span style="color: deepskyblue;">首部</span>
##### 伪首部协议字段
- 用于指明上层协议是TCP还是UDP
  - 17 → UDP
  - 6 → TCP
##### 首部&伪首部' <span style="color: orange;">区别
核心区别：
- **伪首部**是为了计算校验和而设计的临时结构，<span style="color:green;">不参与</span>实际传输
  - 包含了IP层的信息(源IP和目的IP)
- **首部**是**实际传输**数据包的组成部分
  - 首部不包含~