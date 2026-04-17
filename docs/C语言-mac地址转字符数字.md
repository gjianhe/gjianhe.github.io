# C语言-mac地址转字符数字

```c
int main(void)
{
    int index = 0;
    int i = 0;
    uint8_t des_str[20] = {0,};
    uint8_t mac[6] = {0x02, 0x02, 0x84, 0x6A, 0x96, 0x00};
    memset(des_str, 0, sizeof(des_str));

    for (i = 0; i < sizeof(mac); i++)
    {
        des_str[index++] = "0123456789ABCDEF"[(mac[i] >> 4) & 0xf];
        des_str[index++] = "0123456789ABCDEF"[(mac[i] >> 0) & 0xf];
        des_str[index++] = ' ';
    }

    printf("%s\n", des_str);
    return 0;
}
```