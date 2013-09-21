---
layout: post
title:  "bottom half"
date:   2013-09-22 00:16:00
categories: ip driver
---


###Bottom Half     
> bottom half是指将系统中断中一些次要的任务延迟处理，主要是因为中断处理大多会关闭当前cpu的中断响应，如果任务耗时的话，会严重影响系统的实时性能。之所以叫bottom half，因为这部分程序多与中断有关，并且多半在中断处理后运行，相当于中断的后续处理部分，区别是这时cpu的中断是打开的              

早期linux的这种机制称为BH，已经被淘汰。现在内核提供三种与BH相似的机制，分别是:     

> 1. softirq    
>    softirq在interrupt context中运行，不允许block或sleep。同一种类型的softirq可以在不同的cpu上同时运行     
> 2. tasklet       
>    tasklet通过softirq机制实现，所以也是在interrupt context中运行。但增加了限制，同一种类型的tasklet只能同时允许运行一个实例，即使不同的cpu也不行      
> 3. work queue               
>    work queue是与softirq完全不同的机制，通过kernel thread实现。在process context中运行，因此可以sleep       


###softirqd
>  softirq的处理时间过长的话，可以reactive，即重新调度。但是这样持续允许的话，其他的进程将无法得到cpu，形成饥饿状态。所以内核提供了专门处理reactive的softirq，即softirqd。softirqd是per cpu的，每个cpu都运行一个softirqd的实例
