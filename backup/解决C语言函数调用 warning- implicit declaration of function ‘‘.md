> 本文截取自 https://blog.csdn.net/xiaobai729/article/details/127138269

一般常见原因是先写了主函数，然后在主函数中调用其他函数。
解决办法：
- 将主函数写在最前
- 使用函数声明

```c
#include <stdio.h>
int main(){
    void test();
    return 0;
}
void test(){
    printf("Hello, world!\n");
}
``` 