# STM32-使用SysTick 实现延时

## 第一种方式

```c
void LL_uDelay(uint32_t Delay)
{
    uint32_t ticks;
    uint32_t told, tnow, tcnt = 0;
    uint32_t reload = SysTick->LOAD;

    ticks = Delay * reload / 1000U;
    told = SysTick->VAL;

    while (1)
    {
        tnow = SysTick->VAL;
        if (tnow != told)
        {
            if (tnow < told)
            {
                tcnt += told - tnow;
            }
            else
            {
                tcnt += reload - tnow + told;
            }
            told = tnow;
            if (tcnt >= ticks)
            {
                break;
            }
        }
    }    
}

```

## 第二种方式

```c
void LL_uDelay( __IO uint32_t us)
{
    uint32_t i;
    SysTick_Config(SystemCoreClock/1000000);

    for (i=0; i<us; i++)
    {
        while ( !((SysTick->CTRL)&(1<<16)) );
    }

    SysTick->CTRL &=~SysTick_CTRL_ENABLE_Msk;
}


void LL_mDelay( __IO uint32_t ms)
{
    uint32_t i;
    SysTick_Config(SystemCoreClock/1000);

    for (i=0; i<ms; i++)
    {
        while ( !((SysTick->CTRL)&(1<<16)) );
    }

    SysTick->CTRL &=~ SysTick_CTRL_ENABLE_Msk;
}
```