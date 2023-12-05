## Chisel基础
### Vec
- Vec代表一系列相同的chisel type的collection，可以通过index访问，类似于array
- 创建Vec通过调用constructor来实现，两个参数，一个是向量中元素的个数，另一个是向量中元素的类型。向量同样也需要封装到一个Wire
```scala
val v = Wire(Vec(3, UInt(4.W)))

```
- Combinational Vec
- 