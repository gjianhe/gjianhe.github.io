# C语言-`array[offset]` 和 `offset[array]` 为什么是等效的？

## 一段代码

```c
#include <stdio.h>

int main(void)
{
    int array[3] = { 0, 1, 2 };
    printf("%d,%d,%d", *array, array[1], 2[array]);
    return 0;
}
```

输出结果：

```
0,1,2
```

## `2[array]` 是什么鬼？

`*array` 等价于 `array[0]` 或者说 `array` 等价于 `&array[0]`， 这个不用解释了吧。 那` 2[array]` 是什么鬼？ 从输出结果不难猜出 `2[array]` 等价于 `array[2]`， 但---这是为什么？

## 方括号的本质

方括号(square brackets)`[]`操作符的本质是什么呢? 编译器在分析表达式`A[B]` 时，方括号在语义上界定了参数 `A` 和 `B`， 然后将分离的参数组合成表达式` (*((A)+(B)))`，本质上仍然是指针的加法。 所以`array[2]`等价于`(*((array)+(2)))`， 而 `2[array]` 等价于`(*((2)+(array)))`

## 参考链接

- [The C Programming Language](https://www.open-std.org/JTC1/SC22/WG14/www/docs/n1516.pdf)的 6.5.2.1 Array subscripting

