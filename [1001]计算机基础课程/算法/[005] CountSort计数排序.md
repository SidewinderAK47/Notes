## 计数排序

复杂度为O(n)

特点：运行过程中没有比较，将一个待排序的数组进行排序， 将排序之后的结果放到另一个新的数组中。

计数排序算法：扫描待排序的数组一趟，统计待排序数组中有多少个记录的值比该记录的值小。

1.首先遍历原始数组，统计某个数字出现的次数

2.求出不比数字j大的元素个数

3.最后遍历原始数组，找到不大于当前元素的个数x，在新数组【x-1】位置放置当前元素，count个数减1；

```C++
//其中inputArr为输入，outputArr为输出，inputLen为元素个数，maxKey为最大元素
void CountSort(int inputArr[], int outputArr[], int inputLen, int maxKey)
{
    int j;
    int *c = new int[maxKey+1];
    memset(c, 0, (maxKey+1) * sizeof(int));
    for (j = 0; j < inputLen; j++)
	{
        c[inputArr[j]]++;                    //求出重复的数字出现多少次
	}
    for (j = 1; j <= maxKey; j++) 
	{
        c[j] += c[j - 1];            //求出不比数字j大的元素的个数，包括相等的
	}
    for (j = inputLen - 1; j >= 0; j--)  //注意此处不能写成for(int j = 0; j < inputLen; j++)，否则会造成排序算法的不稳定
    {
        outputArr[c[inputArr[j]] - 1] = inputArr[j];
        c[inputArr[j]]--;
    }
    delete []c;
}

```

