---
layout: post
title:  "读书笔记：UNIX 网络编程 - I/O"
subtitle: "I/O 模型与 Java IO"
date:   2020-08-21 8:00:00
author: "Shoukai Huang"
header-img: 'qjy1xw2zw.hn-bkt.clouddn.com/ee775f9df451839ea5775737886f12ae.jpg'
header-mask: 0.4
tags: 读书笔记
---

（同事讲解与讨论后，重新梳理I/O模型与Java网络编程）

# I/O模型

## Unix下可用的5种I/O模型的基本区别

* 阻塞式I/O
* 非阻塞式I/O
* I/O复用（select和poll）
* 信号驱动式I/O（SIGIO）
* 异步I/O（POSIX的aio_系列函数）

## 阻塞式I/O模型

最流行的I/O模型是阻塞式I/O（blocking I/O）模型，默认情形下，所有套接字都是阻塞的。

![](http://qjy1xw2zw.hn-bkt.clouddn.com/276bc1cdf18b452ffb67eb2879acc39a.jpg)

Blocking IO 是直到数据真的全拷贝至 User Space 后才返回。

```JAVA
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

public class EchoServer {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(8080);
        while (true) {
            System.out.println("start accept");
            Socket socket = serverSocket.accept();
            System.out.println("new conn: " + socket.getRemoteSocketAddress());

            new Thread(() -> {
                try {
                    BufferedReader reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
                    String msg;
                    while ((msg = reader.readLine()) != null) {
                        if (msg.equalsIgnoreCase("quit")) {
                            reader.close();
                            socket.close();
                            break;
                        } else {
                            System.out.println("receive msg: " + msg);
                        }
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }
}
```

Client 实例

```Java
import java.io.IOException;
import java.net.Socket;

public class EchoClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("127.0.0.1", 8080);
        socket.getOutputStream().write("Hello World!".getBytes());
        socket.close();
    }
}

```

BIO最大的缺点就是浪费资源，只能处理少量的连接，线程数随着连接数线性增加，连接越多线程越多。


## 非阻塞式I/O模型

进程把一个套接字设置成非阻塞是在通知内核：当所请求的I/O操作非得把本进程投入睡眠才能完成时，不要把本进程投入睡眠，而是返回一个错误。

![](http://qjy1xw2zw.hn-bkt.clouddn.com/2dea1ee315d6f599cffdcff2a45bf3a7.jpg)

前三次调用recvfrom时没有数据可返回，因此内核转而立即返回一个EWOULDBLOCK错误。第四次调用recvfrom时已有一个数据报准备好，它被复制到应用进程缓冲区，于是recvfrom成功返回。我们接着处理数据。

当一个应用进程像这样对一个非阻塞描述符循环调用recvfrom时，我们称之为轮询（polling）。应用进程持续轮询内核，以查看某个操作是否就绪。这么做往往耗费大量CPU时间，不过这种模型偶尔也会遇到，通常是在专门提供某一种功能的系统中才有。

配置 Socket 为 Non-Blocking 模式，之后不断去 kernel 做 Polling，询问比如读操作是否完成，没完成则 read() 操作会返回 EWOUDBLOCK，需要过一会再来尝试执行一次 read()。这种模式下会消耗大量 CPU。


## I/O复用模型

有了I/O复用（I/O multiplexing），我们就可以调用select或poll，阻塞在这两个系统调用中的某一个之上，而不是阻塞在真正的I/O系统调用上。

![](http://qjy1xw2zw.hn-bkt.clouddn.com/adfb9b465902db692a0a1f3ea0b9efe2.jpg)

我们阻塞于select调用，等待数据报套接字变为可读。当select返回套接字可读这一条件时，我们调用recvfrom把所读数据报复制到应用进程缓冲区。

比较图中内容，I/O复用并不显得有什么优势，事实上由于使用select需要两个而不是单个系统调用，I/O复用还稍有劣势。

与I/O复用密切相关的另一种I/O模型是在多线程中使用阻塞式I/O。这种模型与上述模型极为相似，但它没有使用select阻塞在多个文件描述符上，而是使用多个线程（每个文件描述符一个线程），这样每个线程都可以自由地调用诸如recvfrom之类的阻塞式I/O系统调用了。

Java NIO，New IO，JDK1.4开始支持，内部是基于多路复用的IO模型。很多人认为Java的NIO是Non-Blocking IO的缩写，其实并不是。

使用NIO则多条连接的数据准备阶段会阻塞在select上，数据从内核空间拷贝到用户空间依然是阻塞的。

因为第一阶段并不是连接本身处于阻塞阶段，所以通常来说NIO也可以看作是同步非阻塞IO。

Server 示例

```Java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.nio.charset.StandardCharsets;
import java.util.Iterator;
import java.util.Set;

public class EchoServer {
    public static void main(String[] args) throws IOException {
        // 获取选择器
        Selector selector = Selector.open();
        // 获取通道 （其实这里的通道类似于之前的 socket连接）
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
        serverSocketChannel.bind(new InetSocketAddress(8080));
        // 切换非阻塞模式
        serverSocketChannel.configureBlocking(false);
        // 将accept事件绑定到selector上
        serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

        while (true) {
            // 阻塞在select上
            selector.select();
            Set<SelectionKey> selectionKeys = selector.selectedKeys();
            // 遍历selectKeys
            Iterator<SelectionKey> iterator = selectionKeys.iterator();
            while (iterator.hasNext()) {
                SelectionKey selectionKey = iterator.next();
                // 如果是accept事件
                if (selectionKey.isAcceptable()) {
                    ServerSocketChannel ssc = (ServerSocketChannel) selectionKey.channel();
                    SocketChannel socketChannel = ssc.accept();
                    System.out.println("accept new conn: " + socketChannel.getRemoteAddress());
                    socketChannel.configureBlocking(false);
                    socketChannel.register(selector, SelectionKey.OP_READ);
                } else if (selectionKey.isReadable()) {
                    SocketChannel socketChannel = (SocketChannel) selectionKey.channel();
                    ByteBuffer buffer = ByteBuffer.allocate(1024);
                    // 如果是读取事件，将数据读入到buffer中
                    int length = socketChannel.read(buffer);
                    if (length > 0) {
                        buffer.flip();
                        byte[] bytes = new byte[buffer.remaining()];
                        // 将数据读入到byte数组中
                        buffer.get(bytes);
                        // 换行符会跟着消息一起传过来
                        String content = new String(bytes, StandardCharsets.UTF_8).replace("\r\n", "");
                        if (content.equalsIgnoreCase("quit")) {
                            selectionKey.cancel();
                            socketChannel.close();
                        } else {
                            System.out.println("receive msg: " + content);
                        }
                    }
                }
                iterator.remove();
            }
        }
    }
}
```

Client 示例与阻塞IO示例相同

NIO的使用方式就有点复杂了，但是一个线程就可以处理很多连接。

首先，需要注册一个ServerSocketChannel并把它注册到selector上并监听accept事件，然后accept到连接后会获取到SocketChannel，同样把SocketChannel也注册到selector上，但是监听的是read事件。

NIO最大的优点，就是一个线程就可以处理大量的连接，缺点是不适合处理阻塞性任务，因为阻塞性任务会把这个线程占有着，其它连接的请求将得不到及时处理。


## 信号驱动式I/O模型

我们也可以用信号，让内核在描述符就绪时发送SIGIO信号通知我们。我们称这种模型为信号驱动式I/O（signal-driven I/O）

![](http://qjy1xw2zw.hn-bkt.clouddn.com/123ac2c4173bb1a0f1d14404908842f2.jpg)

我们首先开启套接字的信号驱动式I/O功能，并通过sigaction系统调用安装一个信号处理函数。该系统调用将立即返回，我们的进程继续工作，也就是说它没有被阻塞。当数据报准备好读取时，内核就为该进程产生一个SIGIO信号。我们随后既可以在信号处理函数中调用recvfrom读取数据报，并通知主循环数据已准备好待处理，也可以立即通知主循环，让它读取数据报。

无论如何处理SIGIO信号，这种模型的优势在于等待数据报到达期间进程不被阻塞。主循环可以继续执行，只要等待来自信号处理函数的通知：既可以是数据已准备好被处理，也可以是数据报已准备好被读取

## 异步I/O模型

异步I/O（asynchronous I/O）由POSIX规范定义。演变成当前POSIX规范的各种早期标准所定义的实时函数中存在的差异已经取得一致。一般地说，这些函数的工作机制是：告知内核启动某个操作，并让内核在整个操作（包括将数据从内核复制到我们自己的缓冲区）完成后通知我们。这种模型与前一节介绍的信号驱动模型的主要区别在于：信号驱动式I/O是由内核通知我们何时可以启动一个I/O操作，而异步I/O模型是由内核通知我们I/O操作何时完成。

![](http://qjy1xw2zw.hn-bkt.clouddn.com/49840029bfc75e3b355558ca412b50ca.jpg)

我们调用aio_read函数（POSIX异步I/O函数以aio_或lio_开头），给内核传递描述符、缓冲区指针、缓冲区大小（与read相同的三个参数）和文件偏移（与lseek类似），并告诉内核当整个操作完成时如何通知我们。该系统调用立即返回，而且在等待I/O完成期间，我们的进程不被阻塞。本例子中我们假设要求内核在操作完成时产生某个信号。该信号直到数据已复制到应用进程缓冲区才产生，这一点不同于信号驱动式I/O模型。

AIO，Asynchronous IO，异步IO，JDK1.7开始支持，算是一种比较完美的IO，Windows下比较成熟，但Linux下还不太成熟。

```Java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.AsynchronousServerSocketChannel;
import java.nio.channels.AsynchronousSocketChannel;
import java.nio.channels.CompletionHandler;
import java.nio.charset.StandardCharsets;
import java.util.concurrent.Future;

public class AioEchoServer {

    public static void main(String[] args) throws IOException {
        AsynchronousServerSocketChannel serverSocketChannel = AsynchronousServerSocketChannel.open();
        serverSocketChannel.bind(new InetSocketAddress(8080));
        // 监听accept事件
        serverSocketChannel.accept(null, new CompletionHandler<AsynchronousSocketChannel, Object>() {
            @Override
            public void completed(AsynchronousSocketChannel socketChannel, Object attachment) {
                try {
                    System.out.println("accept new conn: " + socketChannel.getRemoteAddress());
                    // 再次监听accept事件
                    serverSocketChannel.accept(null, this);

                    // 消息的处理
                    while (true) {
                        ByteBuffer buffer = ByteBuffer.allocate(1024);
                        // 将数据读入到buffer中
                        Future<Integer> future = socketChannel.read(buffer);
                        if (future.get() > 0) {
                            buffer.flip();
                            byte[] bytes = new byte[buffer.remaining()];
                            // 将数据读入到byte数组中
                            buffer.get(bytes);

                            String content = new String(bytes, StandardCharsets.UTF_8);
                            // 换行符会当成另一条消息传过来
                            if (content.equals("\r\n")) {
                                continue;
                            }
                            if (content.equalsIgnoreCase("quit")) {
                                socketChannel.close();
                                break;
                            } else {
                                System.out.println("receive msg: " + content);
                            }
                        }
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }

            @Override
            public void failed(Throwable exc, Object attachment) {
                System.out.println("failed");
            }
        });

        // 阻塞住主线程
        System.in.read();
    }
}

```

Client 示例与阻塞IO示例相同

AIO的使用方式不算太复杂，默认会启一组线程来处理用户的请求，而且如果在处理阻塞性任务，还会自动增加新的线程来处理其它连接的任务。

首先，创建一个AsynchronousServerSocketChannel并调用其accept方法，这一步相当于监听了accept事件，在收到accept事件后会获取到AsynchronousSocketChannel，然后就可以在回调方法completed()里面读取数据了，当然也要继续监听accept事件。

AIO最大的优点，就是少量的线程就可以处理大量的连接，而且可以处理阻塞性任务，但不能大量阻塞，否则线程数量会膨胀。


## 各种I/O模型的比较

图中对比了上述5种不同的I/O模型。可以看出，前4种模型的主要区别在于第一阶段，因为它们的第二阶段是一样的：在数据从内核复制到调用者的缓冲区期间，进程阻塞于recvfrom调用。相反，异步I/O模型在这两个阶段都要处理，从而不同于其他4种模型。

![](http://qjy1xw2zw.hn-bkt.clouddn.com/3d187d9edbfab0d1256c6031e683a57c.jpg)

## 同步I/O和异步I/O对比

POSIX把这两个术语定义如下：

* 同步I/O操作（synchronous I/O opetation）导致请求进程阻塞，直到I/O操作完成；
* 异步I/O操作（asynchronous I/O opetation）不导致请求进程阻塞。

根据上述定义，我们的**前4种模型——阻塞式I/O模型、非阻塞式I/O模型、I/O复用模型和信号驱动式I/O模型都是同步I/O模型，因为其中真正的I/O操作（recvfrom）将阻塞进程。只有异步I/O模型与POSIX定义的异步I/O相匹配**。

示例代码：https://github.com/shoukai/java-journey

# 参考

* [UNIX网络编程 卷1：套接字联网API（第3版）]()
* [彤哥说netty系列之Java BIO NIO AIO进化史](https://juejin.im/post/6844904000026853389)
* [IO 多路复用](https://nextfe.com/io-multiplexing/)
* [rymuscle Java NetIO](https://github.com/rymuscle/JavaNetIO)