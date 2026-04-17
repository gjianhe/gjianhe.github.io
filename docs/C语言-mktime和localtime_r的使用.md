# C语言-mktime和localtime_r的使用

## 注意事项
1. 在mdk中使用gmtime没有效果，必须使用localtime_r
2. localtime_r是localtime线程安全的变体
3. 在mdk中，localtime_r的返回结果跟gmtime一样，并不会自动加上时区（它也不知道）


```c
#include "string.h"
#include "time.h"


void test(void)
{
    struct tm date;
    time_t timestamp;
    // // 2026-02-28 15:31:56 周六
    date.tm_sec = 56;
    date.tm_min = 31;
    date.tm_hour = 15 - 8;  // 东八区
    date.tm_mday = 28;
    date.tm_mon = 2 - 1;    // 从0开始计数，0是1月份
    date.tm_year = 2026 - 1900; // 从1900年后开始计数
    // 日期时间转时间戳，
    // 调用mktime后，tm_wday = 6和tm_yday = 58以及tm_isdst = -1会被自动填充
    // timestamp = 1772263916
    timestamp = mktime(&date);
    //
    memset((void *)&date, 0, sizeof(date));
    // 时间戳转日期时间，
    localtime_r(&timestamp, &date);
}
```
