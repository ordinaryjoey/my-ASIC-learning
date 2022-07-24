# AHB
Transfers can be:  
• Single.  
• Incrementing bursts that do not wrap at address boundaries.  
• Wrapping bursts that wrap at particular address boundaries.  
Every transfer consists of:  
**Address phase** One address and control cycle.  
**Data phase** One or more cycles for the data.  

When the Subordinate is initially selected, it must also monitor the status  
of HREADY to ensure that the previous bus transfer has completed,  
before it responds to the current transfer.  

When the Subordinate is initially selected, it must also monitor the status 
of HREADY to ensure that the previous bus transfer has completed, 
before it responds to the current transfer.

This overlapping of 
address and data is fundamental to the pipelined nature of the bus and enables high-performance operation while 
still providing adequate time for a Subordinate to provide the response to a transfer.

The interconnect is responsible for 
combining the HREADYOUT signals from all Subordinates to generate a single HREADY signal that is used to 
control the overall progress.

## HTRANS ##  
#### IDLE ####  
Subordinates 必须用OKAY回应IDLE。
#### Busy transfer
Manager可以在burst传输中插入busy cycle。
Busy cycle 可以用于终结不定长burst。
Busy cycle 不能用来终结fixed burst or wrapping
Single transfer后面不能接busy，只能接IDLE或另一个 NONSEQ transfer。
Subordinates 必须用OKAY回应BUSY。
虽然在Busy cycle，manager需要提供下一个传输的address和ctrl signal，但是在busy的data phase，也就是下一个NEQ transfer的address phase时，subordinate返回的HRDATA应被manager忽略。

![image](https://user-images.githubusercontent.com/34599267/180637282-3ca33630-208e-4b66-84aa-0e8fa38f5532.png)

## Lock ##
比如经典的read-modify-write问题，其本质是保持一个对内存read和write访问的原子性问题，也就是说内存的读和写的访问不能被打断。对该问题的解决可以通过硬件、软件或者软硬件结合的方法来进行。早期的ARM CPU给出的方案就是依赖硬件：SWP这个汇编指令执行了一次读内存操作、一次写内存操作，但是从程序员的角度看，SWP这条指令就是原子的，读写之间不会被任何的异步事件打断。具体底层的硬件是如何做的呢？这时候，硬件会提供一个lock signal，在进行memory操作的时候设定lock信号，告诉总线这是一个不可被中断的内存访问，直到完成了SWP需要进行的两次内存访问之后再clear lock信号。
After a locked transfer, it is recommended that the Manager inserts an IDLE transfer.

Subordinates that can be accessed by more than one Manager, for example, 
a Multi-Port Memory Controller (MPMC) must implement the HMASTLOCK signal

It is required that all transfers in a locked sequence are to the same Subordinate address region.

Q:
### IDLE transfer 时，置起HSEL有什么用？
### After lock transfer, why suggest that manager insterts an IDLE transfer???

