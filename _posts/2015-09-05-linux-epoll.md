---
title: linux-epoll
description:
categories:
 - linux
tags:
---

几种IO模型的触发方式
select()模型是水平触发模式，信号驱动IO是边缘触发模式，epoll()模型即支持水平触发，也支持边缘触发，默认是水平触发 

epoll的两种触发方式
Level_triggered(水平触发)：当被监控的文件描述符上有可读写事件发生时，epoll_wait()会通知处理程序去读写。如果这次没有把数据一次性全部读写完(如读写缓冲区太小)，那么下次调用 epoll_wait()时，它还会通知你在上次没读写完的文件描述符上继续读写

Edge_triggered(边缘触发)：当被监控的文件描述符上有可读写事件发生时，epoll_wait()会通知处理程序去读写。如果这次没有把数据全部读写完(如读写缓冲区太小)，那么下次调用epoll_wait()时，它不会通知你，也就是它只会通知你一次，直到该文件描述符上出现第二次可读写事件才会通知你

由此可见，水平触发时如果系统中有大量你不需要读写的就绪文件描述符，而它们每次都会返回，这样会大大降低处理程序检索自己关心的就绪文件描述符的效率，而边缘触发，则不会充斥大量你不关心的就绪文件描述符，从而性能差异，高下立见

关于两种触发方式的难点整理
accept函数调用时使用哪种触发方式？ 
accept除了在某些特殊情况下(如连接已完成三次握手、进入就绪队列中等待被accept，然后客户端又断开)，一调用就直接返回，因此讨论起fd的阻塞非阻塞无意义，nigix的实现中，accept函数调用使用水平触发的socket fd，是出于对丢失连接的考虑（边缘触发时，accept只会执行一次接收一个连接，内核不会再去通知有连接就绪），当然，若使用while循环包裹住accept也是能够实现接受所有连接，稍显复杂，不如直接使用水平触发简单直观
为何边缘触发不用处理EINTR 
socket接口并不是被信号中断，只是调用了睡眠，发生信号睡眠会被唤醒通知进程，然后socket接口选择主动退出，这样做可以避免一直阻塞在那里，有退出的机会。非阻塞时不会调用睡眠，而ET使用的事非阻塞socket fd
为何ET使用的是非阻塞fd 
由于边缘触发的模式，每次epoll_wait返回就绪的fd，必须读完读取缓冲区里的所有数据（直至接收数据返回EAGAIN），必须套上while循环，此时若使用阻塞的fd，当读取完缓冲区里的数据后，接受数据过程会阻塞，从而无法接受新的连接，而水平触发由于只接受一次数据（既然fd就绪可读，第一次调用并不会阻塞），然后若缓冲区仍有数据，等待下次epoll_wait返回
关于epoll_wait的写就绪事件 
水平触发： 
若socket fd 可写，每次epoll_wait都会返回该fd，会造成CPU占用100%的毛病， 
解决方法

需要向socket写数据的时候才把socket加入epoll，等待可写事件。 
接受到可写事件后，调用write或者send发送数据。 
当所有数据都写完后，把socket移出epoll。
开始不把socket加入epoll，需要向socket写数据的时候，直接调用write或者send发送数据。如果返回 EAGAIN，把socket加入epoll，在epoll的驱动下写数据，全部数据发送完毕后，再移出epoll
后一种方式在send数据较少时，能够减少epoll_wait的调用，性能有所提升

边缘触发： 
只有在socket fd由不可写到可写状态时，epoll_wait才回返回该socket fd,因此，需要向socket写数据的时候，直接循环调用write或者send，直至finish或者返回EAGAIN，若返回EAGAIN则注册该socket fd的EPOLLOUT事件，等待下次epoll_wait触发发送数据，发送完成后再注册该socket fd的EPOLLIN事件，即ATM模式

EPOLL事件的触发时机

EPOLLIN 
ET模式：每次EPOLL_CTL_ADD或EPOLL_CTL_MOD时，如果加入前，就是可读状态，那么加入后会触发1次 ，不管sock缓冲是否读完，只要对方有send或connect或close或强退，就会触发EPOLLIN（ET/LT是针对一次socket fd就绪的，即一次fd就绪后有数据没读完/没写完，是否还会通知，所以该连接新的数据到来，该事件会触发） 
LT模式：只要socket可读，就会一直触发EPOLLIN
EPOLLOUT 
ET模式： 每次EPOLL_CTL_ADD或EPOLL_CTL_MOD时，如果加入前，就是可写状态，那么加入后会触发1次 ，如果EPOLLOUT与EPOLLIN一起注册，不管sock发送缓冲是否从满变不满，只要socket发送是不满的，那么每次EPOLLIN触发时，都会触发EPOLLOUT，即获取到不被期望的写事件，这也是为什么要使用ATM模式的原因 
LT模式：只要socket可写，就会一直触发EPOLLOUT
简单来说，ET模式下，只要监听了EPOLLIN和EPOLLOUT，socket的每次动作（包括close与不close直接强退），都会触发1次， 与读写缓冲的状态无关。
