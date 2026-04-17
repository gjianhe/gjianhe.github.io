
# C语言-插入排序

```c
#include <stdio.h>

void insertion_sort(int A[], int len)
{
    int key = 0;
    int i, j;

    for (i = 1; i < len; i++)
    {
        key = A[i];
        j = i - 1;

        while (j >= 0 && A[j] > key)
        {
            A[j + 1] = A[j];
            j--;
        }

        A[j + 1] = key;
    }
}


int main(void)
{
    int i;
    int a[] = {5, 2, 4, 6, 1, 3};
    insertion_sort(a, 6);

    for (i = 0; i < 6; i++)
    {
        printf("%d ", a[i]);
    }

    return 0;
}
```
