# C语言-判断一个值是否在范围内

```c
#include <stdint.h>

int is_in_range_8(uint8_t val, uint8_t min, uint8_t max)
{
	if (min <= main)
	{
		return val >= min && val <= max;
	}
	else
	{
		return (val >= min && val <= UINT8_MAX) || (val <= max);
	}
}

int is_in_range_16(uint16_t val, uint16_t min, uint16_t max)
{
	if (min <= main)
	{
		return val >= min && val <= max;
	}
	else
	{
		return (val >= min && val <= UINT16_MAX) || (val <= max);
	}
}

int is_in_range_32(uint32_t val, uint32_t min, uint32_t max)
{
	if (min <= main)
	{
		return val >= min && val <= max;
	}
	else
	{
		return (val >= min && val <= UINT32_MAX) || (val <= max);
	}
}
```