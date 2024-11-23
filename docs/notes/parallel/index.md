---
layout: default
title: Parallel Computing
parent: Notes
nav_order: 1
has_children: True
---
## Instruction level parallelism(ILP)

指令层面的优化是指将互不依赖的指令分配到不同的处理单元上，例如$a = x*x + y*y + z*z$，我们首先将乘法分配到三个unit上，再将加法分配到两个unit上。直觉上，可供分配的unit越多，并行程序性能越好，但这里不是tcs我们不可能造一个无限多unit的计算机(superscalar processor)。Most available ILP is expolited by a processor capable of issuing four instructions per clock. 这也在某种程度上制约了摩尔定律，intel的pentium 4，整数单元和指令解码器ILP=2，transistor继续增加，但已不可期望通过提高ILP获得更好的性能。

另外，dynamic power正比于$capacitive load*voltage^2*frequency$，由于功耗墙的存在，我们也不可能一直提高时钟频率。由于这两个因素，现代CPU一方面增加一些专用的处理单元，一方面增加一些更高层面的并行。软件开发也因此，有了考虑并行的必要。

## Memory access

除开计算速度之外，获取数据的时间也制约了程序整体的性能。Memory access latency, 也就是说我们把数据从内存load到寄存器需要的时间。 降低这项的时间开支，可以使用on-chip cache，一旦出现cold miss，cache会将连续的几个地址中的数据装载进cache line。 如果capacity miss, 剔除掉老的cache. 现代CPU可能使用多级缓存，具体实现非常复杂。 