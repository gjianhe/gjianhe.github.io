
# C语言-strcasecmp()函数

## 函数定义

```c
int strcasecmp (const char *s1, const char *s2);
```

## 概述

C语言中判断字符串是否相等的函数，忽略大小写。s1和s2中的所有字母字符在比较之前都转换为小写。该 `strcasecmp()` 函数对空终止字符串进行操作。函数的字符串参数应包含一个(`'\0'`)标记字符串结尾的空字符。

## 头文件

```c
#include<string.h>
```

## 返回值

| 值   | 意义  |
| --- | --- |
| 小于0 | s1 小于s2 |
| 0   | s1 等于s2 |
| 大于0 | s1大于s2 |

## 例子

```c
#include<string.h>
#include<stdio.h>

void compareLengthOfStr(char const *a, char const *b)
{
    int flag = strcasecmp(a, b);

    if (!flag)
    {
        printf("the length of a equals length of b.\n");
    }
    else if (flag > 0)
    {
        printf("the length of a is greater.\n");
    }
    else
    {
        printf("the length of b is greater.\n");
    }
}
int main()
{
    char const *a = "adjhiog";
    char const *b = "adjHIOG";
    char const *c = "whieHIOUIHF";
    compareLengthOfStr(a, b);
    compareLengthOfStr(a, c);
    return 0;
}
```

输出结果

```
the length of a equals length of b.
the length of b is greater.
```

