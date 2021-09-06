### 数组

#### 剑指53 - I. 在排序数组中查找数字 I

**排序中的搜索问题，首先考虑二分**



#### 剑指 Offer 53 - II. 0～n-1中缺失的数字

“**考点主要还是二分法，题目每个条件都是有用的。所有题都拿来遍历，offer也就遍历到别人那里去了**”

根据题意，数组可以按照以下规则划分为两部分。

- 左子数组： nums[i] = inums[i]=i ;
- 右子数组： nums[i]  != nums[i] ;

缺失的数字等于 “右子数组的首位元素” 对应的索引；因此考虑使用二分法查找 “右子数组的首位元素”

```c++
class Solution 
{
    public:
        int missingNumber(vector<int>& nums) 
        {
            if(nums.size() == 0)
            {
                return -1;
            }

            int left = 0, right = nums.size() - 1;
            while(left <= right)
            {
                int mid = (left + right)/2;
                if(nums[mid] != mid)
                {
                    right = mid - 1;
                }
                else
                {
                    left = mid + 1;
                }
            }
            return left;
        }
};
```



### 优先队列 / 堆

#### 剑指offer41.数据流中的中位数
如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。  

实现：  
1.每插入一个数之前，先判断两个堆的 size() 是否相等。
2.若相等，先将这个数插入大顶堆，然后将大顶堆的 top() 插入小顶堆。这么做可以保证小顶堆的所有数永远大于等于大顶堆的 top()。  
3.若不相等，先将这个数插入小顶堆，然后将小顶堆的 top() 插入大顶堆。这么做可以保证大顶堆的所有数永远小于等于小顶堆的 top()。  
```c++
class MedianFinder 
{
    public:
        /** initialize your data structure here. */
        
        priority_queue<int, vector<int>, less<int> > maxheap;
        priority_queue<int, vector<int>, greater<int> > minheap;

        MedianFinder() {}
        
        void addNum(int num) 
        {
            if(maxheap.size() == minheap.size()) 
            {
                maxheap.push(num);
                minheap.push(maxheap.top());
                maxheap.pop();
            }
            else 
            {
                minheap.push(num);
                maxheap.push(minheap.top());
                minheap.pop();
            }
        }
        
        double findMedian() 
        {
            int maxSize = maxheap.size(), minSize = minheap.size();
            int mid1 = maxheap.top(), mid2 = minheap.top();
            return maxSize == minSize ? ((mid1 + mid2) * 0.5) : mid2;
        }
};
```
队列：先进先出 
优先队列：
1.最大优先队列，无论入队顺序，当前最大的元素优先出队。
2.最小优先队列，无论入队顺序，当前最小的元素优先出队。  