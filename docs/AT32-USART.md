# AT32-USART

## 发送数据

```c
while (usart_flag_get(USART1, USART_TDBE_FLAG) == RESET);
usart_data_transmit(USART1, dat);
```

## 中断接收

### 开启接收中断

```c
usart_interrupt_enable(USART1, USART_RDBF_INT, TRUE);
```

### 中断函数

```c
void USART1_IRQHandler(void)
{
	if (USART1->ctrl1_bit.rdbfien != RESET)
	{
		if (usart_flag_get(USART1, USART_RDBF_FLAG) != RESET)
		{
			uint8_t dat = usart_data_receive(USART1);
		}
	}
}
```