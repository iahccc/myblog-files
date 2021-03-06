---
title: Java线程的创建
cover: 'https://cdn.jsdelivr.net/gh/iahccc/PicBed/img/archive_img.jpeg'
tags: Java线程
categories: Java

---

## 什么是进程
进程是操作系统进行资源分配和调度的基本单位，是操作系统结构的基础。进程是程序的实体，一个进程对应一个程序，而一个程序可以有多个进程。进程是线程的容器，所以线程是跟进程密不可分的。

## 什么是线程
线程是操作系统能够进行运算调度的最小单位，它被包含在进程之中，是进程中的实际运作单位。也就是说一个进程必须包含一个主线程，并且一个进程能并发多个线程，让每条线程分别执行不同的任务。不同的进程使用不同的内存空间，而所有的线程可以共享相同的内存空间。

## 为什么要使用多线程
1. 提高CPU的利用率，提高程序的执行效率。
2. 当遇到耗时的操作时使用线程可以提高程序的响应速度。比如下载时使用一个下载的线程和一个UI更新的线程，就可以实现下载的进度条更新，否则用户在下载的过程中软件界面就会进入到无响应的状态，体验非常不好。
3. 与进程相比，创建一个线程的花销更小。进程的创建包括创建虚拟地址空间等需要大量系统资源，线程的创建基本上只有一个内核对象和一个堆栈。

## 线程的创建

### 创建线程的方式
1. 通过继承Thread类创建。
2. 通过实现Runable接口创建。
3. 使用Callable和Future来创建。
4. 使用线程池创建。

### 通过实现Thread类创建
