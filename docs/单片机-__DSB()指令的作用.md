# 单片机-__DSB()指令的作用

在一些 **ARM** 程序代码中，会用到 `__DSB()` 指令，特别是在一些中断处理函数中。例如：

```c
//中断定时器PIT中断处理函数
void PIT_LED_HANDLER(void)
{
    /* Clear interrupt flag.*/
    PIT_ClearStatusFlags(PIT, kPIT_Chnl_0, kPIT_TimerFlag);
    pitIsrFlag = true;   
    __DSB();
}

```

程序通过中断信号进入中断处理函数时，首先应当清除相应的中断标志位，但有些 **CPU** 的时钟太快，快于中断使用的时钟，就会出现清除中断标志的动作还未完成，CPU就又一次重新进入同一个中断处理函数，导致死循环，`__DSB()`  指令的作用就是避免上述情况的发生。`__DSB()` 的原型是：

```c
__STATIC_FORCEINLINE void __DSB(void)
{
  __ASM volatile ("dsb 0xF":::"memory");
}
```

作为应用程序开发，我们一般不需要详细了解到这一步。按照官方要求做就好了。
