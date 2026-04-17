# STM32-LL库之printf函数重定向

1. 加入以下代码

```c
int fputc(int ch,FILE *f)
{
	LL_USART_TransmitData8(USART1,ch);
	while(!LL_USART_IsActiveFlag_TXE(USART1));//需要等待发送完成
	return(ch);
}
```

> 记得添加 stdio.h 头文件


2. 在MDK中勾选：Use MicroLIB