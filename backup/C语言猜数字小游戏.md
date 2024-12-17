```c
#include <stdio.h>

int correctA(char n[], char guess[]) //判断位置正确的个数 
{
	int count = 0, i;
	for (i = 0; i < 4; i++)
	{
		if (n[i] == guess[i])
		{
			count++;
		}
	}
	return count;
}

int correctB(char n[], char guess[]) // 判断数字正确的个数 
{
	int count = 0, i, j;
	for (i = 0; i < 4; i++)
	{
		for (j = 0; j < 4; j ++)
		{
			if (guess[j] == n[i])
			{
				count++;
			}
		}
	}
	return count - correctA(n, guess);
}
int play(char n[])
{
	char guess[5];
	gets(guess);
	printf("%dA%dB\n", correctA(n, guess), correctB(n, guess));
	if (correctA(n, guess) == 4)
	{
		printf("You win!\n");
		return 1;
	}else
	{
		return 0;
	}
}
char* random(char str[])
{
	int tag;
	do{
		tag = 0;
		int n;
		srand((unsigned)time(NULL));
		n = rand() % 8999 + 1000; // 生成在1000-9999之间的随机数
		
		// 拆分 
		int n1, n2, n3, n4;
		n1 = n / 1000;
		n2 = n / 100 - n1 * 10;
		n3 = n / 10 - n1 * 100 - n2 * 10;
		n4 = n % 10;
		
		// 转成字符数组
		str[0] = n1 + '0';
		str[1] = n2 + '0';
		str[2] = n3 + '0';
		str[3] = n4 + '0';
		str[4] = '\0';
		
		// 判断是否有重复
		int i, j;
		int recur[10] = {0};
		for (i = 0; i < 4; i++)
		{
			for (j = 0; j < 4; j++)
			{
				if (i != j && str[i] == str[j])
				tag = 1; // 有重复 
				printf("[Warning] There are duplicaes of numbers.\n");
				break;
			}
			if (tag) break;
		}
	} while(tag);
	return str;
}

int main()
{
	char str[5];
	char n[5];
	strcpy(n, random(str));
	int i, count = 0;
	puts("<=====Game Start=====>");
	printf("The number you guess: ");
	while (play(n) != 1)
	{
		count++;
		printf("You still have %d chances.\n", 7 - count);
		if (count > 6){
			printf("You have no chance. The answer is %s\n", n);
			break;	
		}

	}
}
``` 