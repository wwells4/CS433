/*
Operating Systems
Project 2
William Wells
     &
Chawon White
2/20/19
*/

#include "stdio.h"
#include <stdlib.h>
#include <pthread.h>
#include <stdint.h>

#define NUM_THREADS 3

void sortArray(int*);
void *validateRow(void *params);
void *validateCol(void *params);
void *validateGrid(void *params);

typedef struct {
	int board[9][9];
	int returnVal;
}board;

int main(void)
{
	board *brd = (board*) malloc(sizeof(board));
	//Valid Board
	int valid[9][9] = { 6, 2, 4, 5, 3, 9, 1, 8, 7,
		5, 1, 9, 7, 2, 8, 6, 3, 4,
		8, 3, 7, 6, 1, 4, 2, 9, 5,
		1, 4, 3, 8, 6, 5, 7, 2, 9,
		9, 5, 8, 2, 4, 7, 3, 6, 1,
		7, 6, 2, 3, 9, 1, 4, 5, 8,
		3, 7, 1, 9, 5, 6, 8, 4, 2,
		4, 9, 6, 1, 8, 2, 5, 7, 3,
		2, 8, 5, 4, 7, 3, 9, 1, 6 };

	//Invalid Board
	int invalid[9][9] = { 1, 2, 3, 4, 5, 6, 7, 8, 9,
		2, 3, 4, 5, 6, 7, 8, 9, 1,
		3, 4, 5, 6, 7, 8, 9, 1, 2,
		4, 5, 6, 7, 8, 9, 1, 2, 3,
		5, 6, 7, 8, 9, 1, 2, 3, 4,
		6, 7, 8, 9, 1, 2, 3, 4, 5,
		7, 8, 9, 1, 2, 3, 4, 5, 6,
		8, 9, 1, 2, 3, 4, 5, 6, 7,
		9, 1, 2, 3, 4, 5, 6, 7, 8 };
	//Initiate Struct Board
	for (int r = 0; r < 9; r++)
	{
		for (int c = 0; c < 9; c++)
		{			//CHANGE BOARD HERE
			//brd->board[r][c] = valid[r][c];
			brd->board[r][c] = invalid[r][c];
		}
	}
	
	pthread_t workers[NUM_THREADS];
	
	int validArray[3] = { 0, 0, 0 };

	pthread_create(&workers[0], NULL, validateRow, brd);
	pthread_create(&workers[1], NULL, validateCol, brd);
	pthread_create(&workers[2], NULL, validateGrid, brd);
	
	for(int i = 0; i < 3; i++)
	{
		void *intVal;
		pthread_join(workers[i], &intVal);
		validArray[i] = (intptr_t)intVal;
	}	

	if(validArray[0] == 1 && validArray[1] == 1 && validArray[2] == 1)
	{
		printf("The Solution is Valid!\n");
	}
	else
	{
		printf("The Solution is Invalid\n");
	}

	return 0;
}

void *validateRow(void *params)
{
	board* brd = (board *)params;
	int rowArray[9] = { 0, 0, 0, 0, 0, 0, 0, 0, 0 };
	int row[9];
	for (int r = 0; r < 9; r++)
	{
		int index = 0;
		//Extract One Row
		for (int c = 0; c < 9; c++)
		{
			row[index] = brd->board[r][c];
			index++;
		}
		//Sort the Row
		sortArray(row);
		//Check The Row
		int valid = 0;
		for (int i = 0; i < 9; i++)
		{
			if (i + 1 == row[i])
			{
				valid = 1;
			}
			else
			{
				valid = 0;
				break;
			}
		}
		rowArray[r] = valid;
	}
	
	for (int i = 0; i < 9; i++)
	{
		if (rowArray[i] != 1)
		{
			printf("Invalid Row at Row %d\n", i + 1);
			pthread_exit((void *)(intptr_t)0);
			
		}
	}
	pthread_exit((void *)(intptr_t)1);
	//return 1;
}

void *validateCol(void *params)
{
	board* brd = (board *)params;
	int colArray[9] = { 0, 0, 0, 0, 0, 0, 0, 0, 0 };
	int col[9];
	for (int c = 0; c < 9; c++)
	{
		int index = 0;
		//Extract One col
		for (int r = 0; r < 9; r++)
		{
			col[index] = brd->board[r][c];
			index++;
		}
		//Sort the col
		sortArray(col);
		//Check The col
		int valid = 0;
		for (int i = 0; i < 9; i++)
		{
			if (i + 1 == col[i])
			{
				valid = 1;
			}
			else
			{
				valid = 0;
				break;
			}
		}
		colArray[c] = valid;
	}

	for (int i = 0; i < 9; i++)
	{
		if (colArray[i] != 1)
		{
			printf("Invalid Col at Col %d\n", i + 1);
			pthread_exit((void *)(intptr_t)0);
			//return 0;
		}
	}
	
	pthread_exit((void *)(intptr_t)1);
//	return 1;
}

void *validateGrid(void *params)
{
	board* brd = (board *)params;
	int gridArray[9] = { 0, 0, 0, 0, 0, 0, 0, 0, 0 };
	int grid[9];
	int gridCount = 0;

	int r = 0;
	int rowSize = 3;
	int c = 0;
	int colSize = 3;
	int index = 0;
	int rowCount = 1;
	
	for (; r < rowSize; r++)
	{
		for (; c < colSize; c++)
		{
			grid[index] = brd->board[r][c];
			index++;
		}
		if (c == 3 && rowCount != 3 || c == 6 && rowCount != 3 || c == 9 && rowCount != 3)
		{
			c = c - 3;
			rowCount++;
		}
		else if (c == 3 && rowCount == 3 || c == 6 && rowCount == 3 || c == 9 && rowCount == 3)
		{
			rowCount = 1;
			sortArray(grid);
			//Validate Grid
			int valid = 0;
			for (int i = 0; i < 9; i++)
			{
				if (i + 1 == grid[i])
				{
					valid = 1;
				}
				else
				{
					valid = 0;
					break;
				}
			}
			gridArray[gridCount] = valid;
			gridCount++;

			//Change 
			if (c == 9 && r == 2 || c == 9 && r == 5 || c == 9 && r == 7)
			{
				c = 0;
				colSize = 3;
				rowSize += 3;
				index = 0;
			}
			else {
				if (r == 8 && c == 9)
				{
					break;
				}
				r = r - 3;
				colSize += 3;
				index = 0;
			}
		}
	}

	for (int i = 0; i < 9; i++)
	{
		if (gridArray[i] != 1)
		{
			printf("Invalid grid at grid %d\n", i + 1);
			pthread_exit((void *)(intptr_t)0);
			//return 0;
		}
	}
	pthread_exit((void *)(intptr_t)1);
	//return 1;
}

void sortArray(int *arrayToSort)
{
	int k, key, j;
	for (k = 1; k < 9; k++)
	{
		key = arrayToSort[k];
		j = k - 1;
		while (j >= 0 && arrayToSort[j] > key)
		{
			arrayToSort[j + 1] = arrayToSort[j];
			j = j - 1;
		}
		arrayToSort[j + 1] = key;
	}
}
