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


Q:
### IDLE transfer 时，置起HSEL有什么用？
