# C语言-goto语句



`goto`语句又叫无条件转移语句，先看一个例子：

```C
#include <stdio.h>
void main()
{
    if ( 1 )
    {
        goto gotoflag;
    }
    printf( "hello " );
    gotoflag: printf( "nihao" );
}
```

输出：
> nihao

可以看出在执行 `goto gotoflag` 语句之后直接跳转到`gotoflag:printf("nihao");`

`gotoflag:`为标记行，冒号切记不可省略。

反之如果代码这样子

```c
#include <stdio.h>

void main()
{
    if ( 0 )
    {
        goto gotoflag;
    }
    printf( "hello " );
    gotoflag: printf( "nihao" );
}
```

输出：
> hello nihao

可以看到执行了
>printf("hello ");
>gotoflag:printf("nihao");

这两条语句，`gotoflag:`将没有意义。