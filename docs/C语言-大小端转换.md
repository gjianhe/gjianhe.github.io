# C语言-大小端转换

```c
#include "stdint.h"
#include "stdio.h"
#include "stdlib.h"

int main(void)
{
    uint8_t mode = 1;
    uint8_t *buff = NULL;
    uint32_t file_len = 0;
    FILE *infp = fopen("aa.txt", "rb");
    FILE *outfp = fopen("bb.txt", "wb");
    fseek(infp, 0, SEEK_END);
    file_len = ftell(infp);
    fseek(infp, 0, SEEK_SET);
    buff = (uint8_t *)malloc(file_len + 1);

    if (NULL != buff)
    {
        if (file_len == fread(buff, 1, file_len, infp))
        {
            if (mode == 0)  // 2字节
            {
                uint8_t *p = buff;
                uint32_t len = file_len >> 1;

                while (len--)
                {
                    fwrite(p + 1, 1, 1, outfp);
                    fwrite(p + 0, 1, 1, outfp);
                    p += 2;
                }
            }
            else            // 4字节
            {
                uint8_t *p = buff;
                uint32_t len = file_len >> 2;

                while (len--)
                {
                    fwrite(p + 3, 1, 1, outfp);
                    fwrite(p + 2, 1, 1, outfp);
                    fwrite(p + 1, 1, 1, outfp);
                    fwrite(p + 0, 1, 1, outfp);
                    p += 4;
                }
            }
        }
    }

    free(buff);
    fclose(infp);
    fclose(outfp);
    return 0;
}
```