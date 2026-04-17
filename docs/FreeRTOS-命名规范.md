
# FreeRTOS-命名规范

## 变量

- _uint32_t_ 类型变量以 _ul_ 为前缀，其中“u”表示“unsigned” ，“l”表示“long”。
- _uint16_t_ 类型变量以 _us_ 为前缀，其中“u”表示“unsigned” ，“s”表示“short”。
- _uint8_t_ 类型变量以 _uc_ 为前缀，其中“u”表示“unsigned” ，“c”表示“char ”。
- 非 stdint 类型的变量以 _x_ 为前缀。例如，BaseType_t 和 TickType_t， 二者分别是可移植层定义的定义类型，主要架构的自然类型或最有效类型， 以及用于保存 RTOS 滴答计数的类型。
- 非 stdint 类型的未签名变量存在附加前缀 _u_。例如， UBaseType_t（未签名 BaseType_t）类型变量以 _ux_ 为前缀。
- _size_t_ 类型变量也带有 _ux_ 前缀。
- 枚举变量以 _e_ 为前缀
- 指针以附加 _p_ 为前缀，例如，指向 uint16_t 的指针将以 _pus_ 为前缀。
- 根据 MISRA 指南，未限定标准 _char_ 类型仅可包含 ASCII 字符， 并以 _c_ 为前缀。
- 根据 MISRA 指南，*char ** 类型变量仅可包含指向 ASCII 字符串的指针， 并以 _pc_ 为前缀。

## 函数

- 文件作用域静态（私有）函数以 _prv_ 为前缀。
- 根据变量定义的相关规定，API 函数以其返回类型为前缀， 并为 _void_ 添加前缀 _v_。
- API 函数名称以定义 API 函数文件的名称开头。例如，在 tasks.c 中定义 vTaskDelete， 并且具有 void 返回类型。
## 宏

- 宏以定义宏的文件为前缀。前缀为小写。例如， configUSE_PREEMPTION 在 FreeRTOSConfig.h 中定义。
- 除前缀外，所有宏均使用大写字母书写，并使用下划线来分隔单词。