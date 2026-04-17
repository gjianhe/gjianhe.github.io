
# 单片机-从Boot跳转到App

```c
void (*SysMemBootJump)(void) = (void (*)(void)) (*((uint32_t *) (BootAddr + 4)));
SysMemBootJump();
```
