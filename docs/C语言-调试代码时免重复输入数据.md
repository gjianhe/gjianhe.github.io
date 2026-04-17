# C语言-调试代码时免重复输入数据

```c
#include<stdio.h>
#include<stdlib.h> 
 
int main()
{
	int a,b;
	
	char str[100];
	//char *str;
	
	freopen("dat.dat","r",stdin);
	//在当前目录下新建个文件名为“dat.dat",里面存放待读入的数据 
	
	scanf("%d %d",&a,&b);//自动读入文件”dat.dat"里面的数据 
	
	printf("%d %d\n",a,b);
	
	printf("%d\n",a+b);
	
	sprintf(str,"%d",a);//第一个参数不懂为什么指针变量不行，只能是数组名。 
	
	printf("%s\n",str);	
	
	return 0;
 }
 
```