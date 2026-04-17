# C语言-遍历目录和获取文件状态

## 全平台通用
```c
#include <sys/types.h>
#include <sys/stat.h>
#include "dirent.h"
#include "stdio.h"

int main(void)
{
    DIR *dir = NULL;
    struct dirent *dp = NULL;
    struct stat stat_buf;
    //
    stat("C:\\Users\\admin\\Desktop\\module\\tmp\\main.c", &stat_buf);

    if (S_ISDIR(stat_buf.st_mode))
    {
        printf("这是目录\n");
    }
    else
    {
        printf("这不是目录\n");
    }

    if (S_ISREG(stat_buf.st_mode))
    {
        printf("这是一般文件\n");
        printf("文件大小=%d\n", stat_buf.st_size);
    }
    else
    {
        printf("这不是一般文件\n");
    }

    dir = opendir("C:\\Users\\admin\\Desktop\\module\\tmp");

    if (dir)
    {
        while (dp = readdir(dir))
        {
            if (strcmp(".", dp->d_name) == 0 || strcmp("..", dp->d_name) == 0)
            {
                continue;
            }

            printf("%s\n", dp->d_name);
        }
    }

    closedir(dir);
    return 0;
}
```

## windows 专享
```c
#include "stdio.h"
#include "io.h"

int main(void)
{
    intptr_t handle;
    struct _finddata_t file;

    if ((handle = _findfirst("D:\\guanjianhe\\devtest\\*.*", &file)) != -1L)
    {
        do
        {
            if (strcmp(".", file.name) == 0 || strcmp("..", file.name) == 0)
            {
                continue;
            }

            if (file.attrib & _A_SUBDIR)
            {
                printf("这是目录,目录名是：%s\n", file.name);
            }
            else
            {
                printf("%s,文件大小=%u\n", file.name, file.size);
            }
        } while (_findnext(handle, &file) == 0);

        _findclose(handle);
    }

    return 0;
}
```
