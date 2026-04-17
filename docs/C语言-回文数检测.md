# C语言-回文数检测



## 回文数概念

“回文”是指正读反读都能读通的句子，它是古今中外都有的一种修辞方式和文字游戏，如“我为人人，人人为我”等。在数学中也有这样一类数字有这样的特征，称为回文数（palindrome number）。 

设 n 是一任意自然数。若将 n 的各位数字反向排列所得自然数 n1 与 n 相等，则称 n 为一回文数。例如，若 n=1234321，则称 n 为一回文数；但若n=1234567，则 n 不是回文数。



## 代码

```c
int reverse(int n)
{
	int reversed = 0;
	while(n>0)
	{
		reversed = 10 * reversed + n % 10;
		n /= 10;
	}
	return reversed;
}

int isPalindrome(int n)
{
	return (n == reverse(n));
}
```