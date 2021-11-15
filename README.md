# my-code
只是我的练习......
扫雷游戏

#define _CRT_SECURE_NO_WARNINGS 1
#include<stdio.h>
#include<stdlib.h>
#include<time.h>

#define ROW 9
#define COL 9
#define ROWS ROW+2
#define COLS COL+2
#define easy_count 10

void initboard(char board[ROWS][COLS],int rows,int cols,char set);
void dispalyboard(char board[ROWS][COLS], int row, int col);
void setmine(char board[ROWS][COLS], int row, int col);
void findmine(char mine[ROWS][COLS], char show[ROWS][COLS], int row, int col);





#include"game.h"
void initboard(char board[ROWS][COLS], int rows, int cols, char set)
{
	int i = 0;
	int j = 0;
	for (i = 0; i < rows; i++)
	{
		for (j = 0; j < cols; j++)
		{
			board[i][j] = set;
		}
	}
}

void dispalyboard(char board[ROWS][COLS], int row, int col)
{
	int i = 0;
	int j = 0;
	for (i = 0; i <= col; i++)
	{
		printf("%d ", i);
	}
	printf("\n");
	for (i = 1; i <= row; i++)
	{
		printf("%d ", i);
		for (j = 1; j <= col; j++)
		{
			printf("%c ", board[i][j]);
		}
		printf("\n");
	}
}

void setmine(char board[ROWS][COLS], int row, int col)
{
	int count = easy_count;
	while (count)
	{
		int x = rand() % row + 1;//1-9
		int y = rand() % col + 1;//生成随机坐标
		if (board[x][y] == '0')
		{
			board[x][y] = '1';
			count--;
		}
	}
}

int get_mine_count(char mine[ROWS][COLS], int x, int y)//定义函数，计算雷
{
	return mine[x - 1][y] +
		mine[x - 1][y - 1] +
		mine[x][y - 1] +
		mine[x + 1][y] +
		mine[x + 1][y + 1] +
		mine[x][y + 1] +
		mine[x - 1][y + 1] +
		mine[x + 1][y + 1] - 8 * '0';//一圈的雷

}
void findmine(char mine[ROWS][COLS], char show[ROWS][COLS], int row, int col)
{
	int x = 0;
	int y = 0;
	int win = 0;
	while (win<row*col-easy_count)//雷排完了的条件
	{
		printf("请输入坐标：>");
		scanf_s("%d%d", &x, &y);
		if (x >= 1 && x <= row && y >= 1 && y <= col)//判断坐标合法性
		{
			if (mine[x][y] == '1')//判断是否踩雷
			{
				printf("很遗憾，你被炸死了！");
				dispalyboard(mine, ROW, COL);//告诉玩家
				break;
			}
			else//没踩雷,计算雷数
			{
				int count = get_mine_count(mine, x, y);//用函数计算
				show[x][y] = count + '0';
				dispalyboard(show, ROW, COL);
				win++;
			}
		}
		else
		{
			printf("输入坐标非法，请重新输入");
		}
	}
	if (win == row * col - easy_count)
	{
		printf("恭喜你排雷成功！\n");
		dispalyboard(mine, ROW, COL);
	}

}



#include "game.h"


void menu()
{
	printf("******************************\n");
	printf("********   1 . play   ********\n");
	printf("********   0 . exit   ********\n");
	printf("******************************\n");

}
void game()
{
	char mine[ROWS][COLS] = { 0 };//11*11的布置好的雷区，数组
	char show[ROWS][COLS] = { 0 };//排查出来的雷
	initboard(mine,ROWS,COLS,'0');//初始化
	initboard(show,ROWS,COLS,'*');
	//dispalyboard(mine, ROW, COL);//隐藏棋盘
	dispalyboard(show, ROW, COL);//展示棋盘
	setmine(mine, ROW, COL);//布置雷
	//dispalyboard(mine, ROW, COL);//隐藏棋盘
	findmine(mine, show, ROW, COL);
}
void test()
{
	int input = 0;
	srand((unsigned int)time(NULL));//随机数
	do
	{
		menu();
		printf("请选择:>");
		scanf_s("%d", &input);
		switch (input)
		{
		case 1:
			game();
			break;
		case 0:
			printf("退出游戏\n");
			break;
		default:
			printf("选择错误，请重新选择！\n");
			break;
		}
	}while (input);
}

int main()
{
	test();
	return 0;
}
