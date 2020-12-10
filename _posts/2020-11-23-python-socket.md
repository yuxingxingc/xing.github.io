---
layout: post
title: python之socket
tags: python socket
---

# 什么是socket

​	SOCKET是网络协议层中的传输层的API（可编程接口）。通俗的讲，我们电脑进程（例如用HTTP，SMTP协议）和另一个电脑的进程进行沟通，就要通过传输层（TCP 或者 UDP） + 网络层（IPv4协议，ICMP协议）+ 链路层协议（PPP，以太网协议）+ 物理层（光纤传输协议，USB物理层等等）来通信。

​	如果小白看到上面这么多协议肯定比较懵，难道我写个程序要懂这么多协议？实际上我们不必要了解这么多，我们只需要去操作SOCKET这个API就能实现不同计算机之间进程的通信了，是不是很方便了？这也是socket存在的意义，你只用把数据交给socket，他就能够将其传送给底层，最后送至对端机器。



# 协议的种类

​	现实生活中，对应不同的使用场景，我们的网络协议制定者规定了不同的传输层协议TCP和UDP协议。例如我们对传输数据的可靠性有要求那么我们可以使用TCP协议，如果我们对传输的数据可靠性没有要求我们可以用UDP协议。

​	再来说网络层的协议，因为时代的发展，IPV4协议一共可以有2^32个地址，这些随着物联网的发展，很可能地址数量不够用！所以科学家又发明了IPV6协议，其一共可以有2^128个地址！

​	为什么我们要谈协议？因为我们在使用socket的时候需要指定我们使用的协议类型。

# python的socket

## 指定地址和协议

- socket的协议族，下面几个常量表示socket支持的几种协议族:
  - socket.AF_UNIX：这个协议通常用于同一个计算机内的进程之间通信。
  - socket.AF_INET: 这个协议通常用于**不同**计算机内的进程之间通信，这个参数的表示形式是`(host, port)`，其中host表示的主机名或者ipv4的地址。
  - socket.AF_INET6: 这个协议通常用于**不同**计算机内的进程之间通信，这个参数的表示形式是`(host, port, flowinfo, scopeid)`，其中host表示的主机名或者**ipv6**的地址。



> ​	Note: 上面是比较常用的协议，还有其他的协议，根据系统类型的不同，支持的协议类型也不一样。例如还有`AF_BLUETOOTH`蓝牙协议，`AF_TIPC `表示TIPC协议等等。



## socket常用的方法

### 创建套接字

```python
import socket

socket.socket(family=AF_INET, type=SOCK_STREAM, proto=0, fileno=None)
# family: 协议族
# type: socket类型，其中SOCK_STREAM是提供面向连接的可靠传输类型（TCP），SOCK_DGRAM是不连续补可靠的传输类型（UDP）
# proto: 协议号，这是指定ip上层的协议类型，例如sctp就是132
# fileno: 如果设置了这个，那我就从文件中读取family，type和proto
```

### 关闭套接字

```python
import socket

socket.close(fd)
```

### 其他功能

```python
import socket

socket.getaddrinfo(host, port, family=0, type=0, proto=0, flags=0)
# 获取一个sock的地址信息
# 返回的格式是(family, type, proto, canonname, sockaddr)

socket.getfqdn([name])
# 获取指定域名，如果name为空则获取本机域名

socket.gethostbyname(hostname)
# 将域名转化为ip地址

socket.getprotobyname(protocolname)
# 将给定协议名字转为常量，例如'TCP'会被转为6
```

​	更多其他功能请参考Python的socket API文档https://docs.python.org/zh-cn/3.7/library/socket.html#other-functions

### socket对象

```python
import socket

socket.accept()
# 接受一个连接。返回一个`(conn, address)`，其中conn是一个socket对象，addr是对端地址。

socket.bind(address)
# 将套接字绑定到一个地址。

socket.close()
# 关闭套接字，释放底层资源。

socket.connect(address)
# 连接到远程addr的套接字。

socket.getpeername()
# 返回远端连接的套接字的地址。

socket.getsockname()
# 返回本地连接的套接字的地址。

socket.listen([backlog])
# 启动一个服务器，并用于接受连接。

socket.makefile(mode='r', buffering=None, *, encoding=None, errors=None, newline=None)
# 返回和套接字关联的文件对象。

socket.send(bytes[, flags])
# 发送数据给对端套接字。

socket.sendfile(file, offset=0, count=None)
# 使用高性能的os.sendfile发送文件，直到文件的EOF为止，返回发送的总字节数。

socket.recv(bufsize[, flags])
# 从套接字获取接受数据。
# bufsize: 是一次最大接受的数据量

socket.recvfrom(bufsize[, flags])
# 从套接字获取接受数据。
# 但是返回的是(bytes, address)，其中bytes是二进制数据

socket.recvmsg(bufsize[, ancbufsize[, flags]])
# 从套接字接收普通数据（至多 bufsize 字节）和辅助数据。
# ancbufsize用来设置辅助数据
# 返回值是：(data, ancdata, msg_flags, address)data就是接受的二进制数据


```

# SOCKET 编程实例

- 1.将客户端输入的字符串返回。

server端

```python
#!/bin/python3
import socket

HOST = ''
PORT = 50007
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:	# 创建一个监听的套接字
    s.bind((HOST, PORT))
    s.listen(1)		# 指定系统允许的暂时未连接的accept的数量
    conn, addr = s.accept()		# 返回一个新连接的套接字，这个和监听的套接字不是同一个
    with conn:
        print('Connected by', addr)
        while True:
            data = conn.recv(1024)
#            if not data: break
            print(repr(data))
            conn.sendall(data)
```

client端口:

```python
#!/usr/bin/python3

import socket

HOST = '10.71.187.231'
PORT = 50007
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.connect((HOST, PORT))
    while True:
        sdata = bytes(input('type to send:\n'), encoding='UTF-8')
        s.sendall(sdata)
        data = s.recv(1024)
        print('Received', repr(data))
```

- 2.支持IPV6和IPV4的版本

server端

```python
#!/bin/python3
import socket

HOST = None
PORT = 50007
s = None

for res in socket.getaddrinfo(HOST, PORT, socket.AF_UNSPEC, socket.SOCK_STREAM, 0, socket.AI_PASSIVE): 
# 如果传入host为None，那么查询所有接口，其中socket.AF_UNSPEC表示不指定协议族，那么ipv6和4都可以查询到
# 如果设置了 AI_PASSIVE 标志,并且 nodename 是 NULL, 那么返回的socket地址可以用于的bind()函数
    af, socktype, proto, canonname, sa = res
    try:
        s = socket.socket(af, socktype, proto)
    except OSError as msg:
        s = None
        continue
    try:
        s.bind(sa)
        s.listen(1)
    except:
        s.close()
        s = None
        continue
    break

if s is None:
    print('Could not open socket')
    sys.exit(1)
conn, addr = s.accept()
with conn:
    print('Connected by', addr)
    while True:
        data = conn.recv(1024)
        if not data: break
        conn.send(data)
```

client端

```python
#!/usr/bin/python3

import socket

HOST = '10.71.187.231'
PORT = 50007
s = None

for res in socket.getaddrinfo(HOST, PORT, socket.AF_UNSPEC, socket.SOCK_STREAM):
        af, socktype, proto, canonname, sa = res
        try:
                s = socket.socket(af, socktype, proto)
        except OSError as msg:
                s = None
                continue
        try:
                s.connect(sa)
        except OSError as msg:
                s.close()
                s = None
                continue
        break
if s is None:
        print('No sock found!')
        sys.exit(1)
with s:
        s.sendall(b'Hello, World!')
        data = s.recv(1024)
print('received', repr(data))
```



