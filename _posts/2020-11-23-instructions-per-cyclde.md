---
layout: post
author: Akira
title: "Instructions Per Cycle"
---

This article tries to clarify some basic concepts concerning to performance of processor. I am always confused about that :< ...

#### Clock Cycle
**Clock cycle**, or generally, **cycle**, is a single electronic pulse of a CPU. Only during the cycle, CPU is able to perform operations such as fetching, decoding, executing, etc... Therefore, the more cycles in a given time of the CPU, the more operations it can perform. Here comes the concept **clock rate**, which is measured by *clock cycles per second*, such as **ARM Cortex-A77@3.0GHZ**, or **Apple A12 Bionic@2.49.GHZ**. 



#### Instruction per cycle

IPC is the instructions CPU can perform in one cycle. Combined with clock rate, we can get instructions per second(IPS), which measures the speed of CPU. Two techs are important in improvement of CPU speed: pipelining, and out-of-order execution. 



#### Pipelining

With pipelining, every part of CPU will be busy. It is a technique of instruction-level parallelism. The mechanism and implementation is complicated, so for more details you might refer to CSAPP chapter 4.  One concept should be mentioned is **pipeline depth**, which is the number of stages one instruction is divided. Although deeper CPU can have higher IPC, we cannot make CPU deeper and deeper. Beyond some point, adding more pipe stages will not help since there can be more control and data hazard. Here comes another tech.



#### Out-of-order Execution

Out-of-order execution can optimize the order of executing instructions in a window by checking hazard to get speedup. Such processor is more well-designed than in-order processor, and need more power. As the **window** become wider, the complexity increases. Therefore, similar to pipeline depth, we cannot make instruction window wider and wider. 



#### Example

Now let's compare **ARM Cortex-A77@3.0GHZ** and **Apple A12 Bionic@2.49.GHZ**. Obviously Apple A12 has lower clock rate. But the two processors' performances are in same level. Why?  ARM might have deeper pipeline depth, but its circuits design might be less complicated than Apple, so Apple have wider instruction window. This is the reason the two processors' performances are similar. 





