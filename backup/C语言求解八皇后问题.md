利用回溯法
```c
#include <stdio.h>
int place[8] = {0};  //保存每行皇后位置
int flag[8] = {0}; // 列标记
int d1[16] = {0}; // 对角线1
int d2[16] = {0}; // 对角线2
int count = 0;

void place_queen(int x, int y) {
    place[x] = y;
    flag[y] = 1;
    d1[x - y + 7] = 1; // 主对角线索引
    d2[x + y] = 1;     // 副对角线索引
}

void remove_queen(int x, int y) {
    flag[y] = 0;
    d1[x - y + 7] = 0;
    d2[x + y] = 0;
}

void print() { // 打印皇后位置
    int i, j;
    int table[8][8] = {0};
    count++; // 计算解的个数
    printf("No. %d\n", count);
    for (i = 0; i < 8; i++) {
        table[i][place[i]] = 1; // 将place数组转为二维数组
    }
    for (i = 0; i < 8; i++) {
        for (j = 0; j < 8; j++) {
            printf("%d ", table[i][j]);
        }
        printf("\n");
    }
    printf("<================>\n");
}

void generation(int x) {
    int y;
    for (y = 0; y < 8; y++) {
        // 判断是否冲突
        if (!(flag[y] || d1[x - y + 7] || d2[y + x])) {
            place_queen(x, y); // 放置皇后
            if (x < 7) {
                generation(x + 1); // 递归
            } else {
                print(); // 放完所有的皇后
            }
            remove_queen(x, y); // 回溯
        }
    }
}

int main() {
    generation(0); // 从第0行开始放置
}
``` 