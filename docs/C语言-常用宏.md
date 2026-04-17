# C语言-常用公共宏

```c
#define GET_BIT(value, bit) ((value)&(1<<(bit)))
// 或者
#define GET_BIT(value, bit) (((value)&(1<<(bit)))>>(bit))
//
#define SET_BIT(value, bit) ((value)|=(1<<(bit)))
//
#define CLEAR_BIT(value, bit) ((value)&=~(1<<(bit)))
//
#define CPL_BIT(value, bit) ((value)^=(1<<(bit)))

```