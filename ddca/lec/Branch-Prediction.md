# 控制依赖处理
- 控制依赖:在流水线中，下一个cycle时用来取指的PC理论上是下一条应该执行的指令的地址，所有指令都依赖于前一条指令
    - 对于当前fetched的指令是非控制指令，如果ISA规定的instruction size确定，很容易计算出来，即nextPC = PC + size，对于X86可能需要在decode阶段知道size
    - 对于控制指令，情况比较复杂，首先如何确定当前指令是否是控制指令，通常这也需要在decode阶段
- 分支类型
主要从在取指时的跳转方向、跳转时PC的选择，对于addr固定在inst中，通常选择         
![Alt text](image/1-1.png)