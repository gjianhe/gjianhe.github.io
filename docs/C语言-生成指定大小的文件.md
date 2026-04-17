
# C语言-生成指定大小的文件

```c
int main(void)
{
    uint8_t *buf = malloc(1024);

    if (buf)
    {
        memset(buf, 1, 1024);
        FILE *fp = fopen("a.bin", "w+b");

        if (fp)
        {
            for (int i = 0; i < 387; i++)
            {
                if (1 != fwrite(buf, 1024, 1, fp))
                {
                    printf("写入错误\n");
                }
            }

            fclose(fp);
        }
        else
        {
            printf("文件打开失败\n");
        }

        free(buf);
    }
    else
    {
        printf("申请内存失败\n");
    }

    return 0;
}
```

