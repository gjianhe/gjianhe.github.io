# C语言-typeof高级用法


```c
#define kfifo_reset(fifo) \
(void)({ \
	typeof((fifo) + 1) __tmp = (fifo); \
	__tmp->kfifo.in = __tmp->kfifo.out = 0; \
})
```

`fifo` 是一个指针，`typeof((fifo) + 1)`是为了防止传参错误，传成一个变量进来。

`(fifo) + 1`是在`fifo`地址的基础上指向下一个`fifo`类型的地址，`typeof((fifo) + 1)`获取到的类型还是`fifo`类型指针，和`typeof(fifo)`的结果是一致的。