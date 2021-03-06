# 面试题准备
##　1. java NIO 与 IO的区别
    
|编号|io|NIO
|:--|--:|--:|
|1|面向流|面向缓冲|
|2|阻塞|非阻塞|
|3|无选择器|有选择器|
1. 第一个最大的区别
    
    >IO是面向流的,NIO是面向缓冲的.

    1. Java IO面向流意味着每次从流中读取一个或者多个字节,直至读取所有的字节,他没有被缓冲在任何地方
    2. 他不能前后移动流中的数据,若需要 移动数据则需要把数据先放入缓冲区

    > Nio是面向缓冲的

    1. Nio的缓冲导向方法略有不同.数据读取到一个它稍后处理的缓冲区,需要时可以在缓冲区中前后移动
    2. 需要检查改缓冲区中包含所有您需要的数据,而且确保当更多的数据读入缓冲区的时候,不要覆盖缓冲区里尚未处理的数据.

2. 阻塞与非阻塞
    
    > 阻塞

    1. Java IO的各种流逝阻塞的,这意味着当一个线程调用read()或write()时,改线程被阻塞,直到全部数据被读取或者完全写入.该线程在此期间不能再干任何事情.
    2. Java Nio的非阻塞模式,使一个线程从某通道发送请求读取数据,但是他仅能得到目前可用的数据,如果目前没有数据可用时,就什么都不会获取.而不是保持线程阻塞,所以直至数据变为可读取之前,改线程可以继续做他的事情.
    3. 非阻塞写.一个线程请求写入一些数据到某通道,但不需要等待他完全写入,这个线程同时还可以处理别的事情.线程通长将非阻塞IO的空闲时间用于在其他通道上执行IO操作,所以一个单独的线程现在可以管理多个输入和输出的通道(channel).

3. 对与NIO的方式需要选择缓冲区是否写满来决定是否读取而对于IO只需要等待即可

4. 一个线程多连接的方案{连接多,发送数据少}
    1. 网络聊天室
    2. p2p网络中,使用一个单独线程来管理你所有的出站连接
5. 典型的一个连接通过一个线程来处理
    1. 少量连接使用非常高的带宽
    2. 一次发送大量数据.