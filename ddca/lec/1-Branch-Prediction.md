# 控制依赖处理
- 控制依赖:在流水线中，下一个cycle时用来取指的PC理论上是下一条应该执行的指令的地址，所有指令都依赖于前一条指令
    - 对于当前fetched的指令是非控制指令，如果ISA规定的instruction size确定，很容易计算出来，即nextPC = PC + size，对于X86可能需要在decode阶段知道size
    - 对于控制指令，情况比较复杂，首先如何确定当前指令是否是控制指令，通常这也需要在decode阶段
- 分支类型
主要从在取指时的跳转方向、跳转时PC的选择以及计算出nextPC的阶段，对于addr固定在inst中，通常在decode阶段可以得到指令的地址，对于间接跳转通常需要早execution阶段得出目标地址         
![Alt text](image/1-1.png)
- 如何处理分支依赖
    - stall流水线知道nextPC计算出来
    - Guess 下一条指令的地址，branch predction
    - 利用分支延迟，分支延迟槽，branch delay slot
    - 利用分支延迟，做一些其他事情，fine-grained multithreading
    - 减小分支延迟指令，predicated execution
    - multipath execution

# 静态分支预测

# 动态分支预测
## Two-Bit Counter Based Predictor

## Two-Level Predictor
与其只看一个level的branch历史，这主要是利用last time分支的历史，不如尝试看2层level的历史，这就是two level的由来，考虑以下两点：

- 一个分支的结果可能与其他分支相关，即`Global branch correlation`
- 一个分支的结果可能与past outcomes of the same branch相关，而不仅仅是"last time"，这主要体现在look at the longer pattern，即`Local branch correlation`。例如，对于`TNTNTNTNTNTNTNTNTNTN`，对于初始化为weakly taken的两位饱和计数器，将会一直在weakly taken和strongly taken之间的状态循环，导致准确度只有50%,如果能观察到到long pattern，就会发现`TNTN`的规律

### Global Branch Correlation 
对于一条分支指令进行预测时，考虑到前面分支指令的执行结果，这种方法称为全局历史(Global history)的分支预测，比如一下程序：
```c
if(aa == 2) /b1/
    aa = 0
if(bb == 2) /b2/
    bb = 0
if(aa != bb) /b3/
```
如果b1和b2执行，b3肯定不会执行

- idea:将分支结果和所有分支的"global T/NT history"联系起来，预测的结果采用GHR的值来预测
- implementation:将"global T/NT history" of all branch记录在Global History Register(GHR)中，然后使用GHR去index Pattern History Table(PHT)这张表，表中记录在最近的过去看到的GHR的value，这个value可能通过2位饱和计数器来存储.
- two level：即GHR + history at that GHR
  
如图所示：

![Alt text](image/1-2.png)
- GHR是个寄存器，用来编码先前分支的方向，这里是最后N个分支的方向

- 对于示例代码，有内外两个循环，
![Alt text](image/1-3.png)