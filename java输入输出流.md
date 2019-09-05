# java输入输出流学习
这四个类属于抽象流类，不能再程序中直接实例化使用，可以使用其派生的类。
```
 字符流
 字符输入流 Write 读操作
 字符输出流 Reader 写操作
 字节流
 字节输入流 inputStream
 字节输出流 OutputStream
 JAVA IO中包含了许多InputStream,outputStream,reader,Writer的子类。这样设计的原因是让每一个类都负责不同的功能。这也就是为什么IO包中有这么多不同的类的缘故，各类用途汇总如下:
    文件访问
    网络访问
    内存缓存访问
    线程内部通信(管道)
    缓冲
    过滤
    解析
    读写文本 (Readers / Writers)
    读写基本类型数据 (long, int etc.)
    读写对象
```
# JAVA IO:inputStreamReader和OutputStreamWriter
IO中的类要么以Stream结尾，要么以Reader或者Writer结尾。
而这两个类的作用，是吧字节流转换成字符流。中间做了数据的转换，类似适配器模式的思想。
```
1. InputStreamReader 会包含一个InputStream,从而可以将该输入字节流转换成字符流，代码例子:

InputStream inputStream = new FileInputStream("c:\\data\\input.txt");
Reader reader = new InputStreamReader(inputStream);
或者
InputStream inputStream = new FileInputStream("c:\\data\\input.txt");
Reader reader = new InputStreamReader(inputStream, "UTF-8"); // 此时该InputStreamReader会将输入的字节流转换成UTF8字符流。

2. OutputStreamWriter会包含一个OutputStream,从而可以将该输出字节流转换成字符流。

    OutputStream outputStream = new FileOutputStream("c:\\data\\output.txt");
    Writer writer = new OutputStreamWriter(outputStream);
```
# Java NIO 概述
channel,Buffer,Selector构成了核心的API，其他组件如pipe,filelock，只不过是与三个核心组件共同使用的工具类。
## Channel 和 Buffer
```
Channel 有点像流。数据可以从Channel读到Buffer中，也可以从Buffer写到Channel中。
    Channel ---> Buffer
    Channel <--- Buffer
    

Channel和Buffer有好几种类型。下面是JAVA NIO中的一些主要的Channel实现：
  . FileChannel
  . DatagramChanel
  . SocketChannel
  . ServerSocketChannel
  这些通道包括了UDP,TCP网络IO，以及文件IO.
 
 以下是Java NIO里关键的Buffer实现：
    ByteBuffer
    CharBuffer
    DoubleBuffer
    FloatBuffer
    IntBuffer
    LongBuffer
    ShortBuffer
   这些Buffer覆盖了你能通过IO发送的基本数据类型：byte, short, int, long, float, double 和 char。
```
# Selector
```
A  Selector allows a single thread to handle multiple Channel's. this is handy if your application has manyty connections(Channels) open, but only has low traffic on each connection. for instance,in a char server.
                    --->chanel1
thread ---->selector--->chanel2
                    --->chanel3

To use a Selector you register the channel's with it.Then you call its select() method. this method will block until there is an event ready for one of the registerd channels. Once the method returns, the thread can then process these events. Examples of event are incoming connnection.date revceived etc.
```

# Channel
```
 Java NIO Channels are similar to streams wite a few differences:
  . You can both read and write to a Channels.Stream are typically one-way(read or write).
  . Channels can be read and write asynchonously.
  . Channels always read to ,or write from ,a Buffer.

Channel的实现
这些是Java NIO中最重要的通道的实现：

FileChannel
DatagramChannel
SocketChannel
ServerSocketChannel
FileChannel 从文件中读写数据。

DatagramChannel 能通过UDP读写网络中的数据。

SocketChannel 能通过TCP读写网络中的数据。

ServerSocketChannel可以监听新进来的TCP连接，像Web服务器那样。对每一个新进来的连接都会创建一个SocketChannel。

实例：
```
# Buffer
Java NIO中的Buffer用于和NIO通道进行交互。如你所知，数据是从通道读入缓冲区，从缓冲区写入到通道中的。

缓冲区本质上是一块可以写入数据，然后可以从中读取数据的内存。这块内存被包装成NIO Buffer对象，并提供了一组方法，用来方便的访问该块内存。
```
Buffer的基本用法
使用Buffer读写数据一般遵循以下四个步骤：
 1. 写入数据到Buffer
 2. 调用flip()方法
 3. 从Buffer中读取数据
 4. 调用clear()方法或者compact()方法

 当向buffer写入数据时，buffer会记录写入了多少数据。一旦要读取数据，需要通过flip()方法将Buffer从写模式切换到读模式。在读模式下，可以读取之前写入到Buffer的所有数据。
 一旦读完了所有的数据，就需要清空缓冲区，让他可以再被写入。有两种方法能清空缓冲区:调用clear()或compact方法。
```
# Selector
```
why use a Selector?
The advantage of using just a single thread to handle multiple channels is that you need less threads to handle the channels. Actually,you can use just one thread to handle all of your channels . Switching between threads is expensive for an operating system,and each thread takes up some resources(memory) in the operating system too. Therefore, the less thread you use,the better.

```