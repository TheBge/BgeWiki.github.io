```c++
/*****************************
*shellSort.h
*****************************/
#include "stdafx.h"
#include <vector>

using namespace std;

template <typename T>
void shellSort(vector<T>& vec)
{
	int gap = vec.size()/2;										// size = 10
	for (; gap != 0; gap = gap/2)								// gap = 5
	{
		shellSort(vec, gap); //以gap步长为参数进行排序
	}
}
template <typename T>
void shellSort(vector<T>& vec, int gap)
{
	int i = gap;												// i = 5
	for (; i < vec.size(); i++) // 从gap 到 vec.size()之间		 //  5 < 10
	{	
		//这种循环交换的思想，省去每次比较都要交换的次数
		T tempValue = vec[i];								    // temp = vec[5]
		int j = i;											 	//  j = i = 5
        														// 5 >= 5 && temp < vec[5-5]
		for (; j >= gap && tempValue < vec[j - gap] ; j = j - gap) //这里其实进行的是倒序循环
		{
			vec[j] = vec[j - gap];								// vec[5] < vec[0]
            													// vec[5] = vec[0]
		}
		vec[j] = tempValue;										// else vec[5] = vec[5]
	}															// i++  i=6
}																// gap = 5/2 = 2
```

