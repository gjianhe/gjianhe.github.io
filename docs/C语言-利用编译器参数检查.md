# C语言-利用编译器参数检查

## 原理

`sizeof(char[1])`在编译阶段不会报错，而`sizeof(char[-1])`会报错

## 用法

```c
#define BUILD_CHECK(condition) ((void)sizeof(char[1 - 2*(!(condition))]))

struct XX
{
	int a;
	int b;
	int c;
};

int main(void)
{
	//如果结构体不等于12个字节则在编译阶段会报错
	BUILD_CHECK((sizeof(struct XX) == 12));
	return 0;
}
```

