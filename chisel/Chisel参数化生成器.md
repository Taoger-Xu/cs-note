## Scala基础
- 使用for循环写移位寄存器
```scala
  val shiftRegister = RegInit(0.U(8.W))
  shiftRegister(0) := 0.U(1.W)
  for(i <- 1 until 8) {
    shiftRegister(i) := shiftRegister(i-1)
  }
```


## 函数式编程
- 用函数来表示硬件，然后把这些硬件组件用函数式编程的方式（即高阶函数）组合到一起
```scala

```