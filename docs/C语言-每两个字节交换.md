# C语言-每两个字节交换

在工作时候，遇到需要把一个文件中所有的字节，每两个字节交换一个位置，例如第一个字节和第二个字节交换位置，第三个和第四个字节交换一下位置，以此类推。

在此特作下记录。

```c
#include "stdio.h"
#include "string.h"
#include "malloc.h"

int main(void)
{
    FILE *fin;
    FILE *fout;
    void *fBuffStart = NULL;
    void *fBuffEnd = NULL;
    unsigned long fileSize = 0;
    unsigned short *pDouble = NULL;
    unsigned char *pSingle = NULL;
    fin = fopen("test.mbp", "rb"); //打开文件
    fseek(fin, 0L, SEEK_END);      //获取字节数
    fileSize = ftell(fin);
    fseek(fin, 0L, SEEK_SET);
    fBuffStart = (unsigned char *)malloc(fileSize); //申请内存
    memset(fBuffStart, 0, fileSize);                //清零
    fread(fBuffStart, 1, fileSize, fin);            //把内容读入到fBuffStart
    fclose(fin);                                    //关闭文件指针
    fBuffEnd = (unsigned char *)fBuffStart + fileSize;
    pDouble = (unsigned short *)fBuffStart;
    fout = fopen("out.dat", "wb"); //打开输出文件

    while (pDouble != fBuffEnd)
    {
        pSingle = (unsigned char *)pDouble;
        fwrite(pSingle + 1, 1, 1, fout);
        fwrite(pSingle, 1, 1, fout);
        pDouble++;
    }

    fclose(fout);     //关闭文件指针
    free(fBuffStart); //释放内存
    return 0;
}
```


```c
#include "stdint.h"
#include "stdio.h"
#include "stdlib.h"
#include <unistd.h>

int main(int argc, char *argv[])
{
    int ch;
    FILE *infile = NULL;
    FILE *outfile = NULL;
    uint8_t *read_buf = NULL;
    uint8_t *tmp_buf = NULL;
    int mode = 0;
    uint32_t file_len = 0;
    uint32_t i = 0;

    while ((ch = getopt(argc, argv, "i:o:m:")) != -1)
    {
        switch (ch)
        {
            case 'i':
            {
                if (!(infile = fopen(optarg, "rb")))
                {
                    if (outfile != NULL)
                    {
                        fclose(outfile);
                    }

                    goto invalid_argument;
                }

                break;
            }

            case 'o':
            {
                if (!(outfile = fopen(optarg, "wb")))
                {
                    if (infile != NULL)
                    {
                        fclose(infile);
                    }

                    goto invalid_argument;
                }

                break;
            }

            case 'm':
            {
                mode = atoi(optarg);

                if (mode != 1 && mode != 2)
                {
                    if (infile != NULL)
                    {
                        fclose(infile);
                    }

                    if (outfile != NULL)
                    {
                        fclose(outfile);
                    }

                    goto invalid_argument;
                }

                break;
            }

            default:
                goto usage;
                break;
        }

        continue;
    invalid_argument:
        fprintf(stderr, "\nInvalid argument: %s\n", optarg);
    usage:
        (void) fprintf(stderr, "\nCopyright (c) 2023 guanjianhe\n");
        (void) fprintf(stderr, "Usage: %s -i <infile> -o <outfile> -m <1 | 2>\n\n", argv[0]);
        return 0;
    }

    if (mode == 0 || infile == NULL || outfile == NULL)
    {
        if (infile != NULL)
        {
            fclose(infile);
        }

        if (outfile != NULL)
        {
            fclose(outfile);
        }

        goto usage;
    }

    //
    fseek(infile, 0, SEEK_END);
    file_len = ftell(infile);
    fseek(infile, 0, SEEK_SET);
    //
    read_buf = (uint8_t *)malloc(file_len + 1);
    tmp_buf = (uint8_t *)malloc(file_len + 1);

    if (read_buf == NULL || tmp_buf == NULL)
    {
        fclose(infile);
        fclose(outfile);

        if (read_buf != NULL)
        {
            free(read_buf);
        }

        if (tmp_buf != NULL)
        {
            free(tmp_buf);
        }

        return 0;
    }

    if (file_len == fread(read_buf, 1, file_len, infile))
    {
        uint32_t len = file_len >> mode;
        uint8_t *p1 = read_buf;
        uint8_t *p2 = tmp_buf;
        uint8_t tmp = 1 << mode;

        while (len--)
        {
            for (i = 0; i < tmp; i++)
            {
                p2[i] = p1[tmp - i - 1];
            }

            p2 += tmp;
            p1 += tmp;
        }

        fwrite(tmp_buf, 1, file_len, outfile);
    }

    fclose(infile);
    fclose(outfile);
    free(read_buf);
    free(tmp_buf);
    return 0;
}
```



