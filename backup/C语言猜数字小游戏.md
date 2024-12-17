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

int main()
{
	char n[] = "1456";
	int i, count = 0;
	while (play(n) != 1)
	{
		count++;
		printf("You still have %d chances.\n", 7 - count);
		if (count > 6) break;
	}

}
``` 