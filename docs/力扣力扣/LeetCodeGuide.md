## 搜索

### 深度优先搜索

​		DFS在搜索到一个新的节点时，立即对该新节点进行遍历，因此遍历需要使用**先入后出的栈**来实现，也可以用与栈等价的**递归**来实现。

​		DFS也可以用来检测环路，记录每个遍历过的节点的父节点，若一个节点被再次遍历且父节点不同，则说明有环。也可以用拓扑排序判断是否有环路，若最后存在入度不为0的点，则说明有环。

​		有时候我们可能需要对已经搜索过的节点进行标记，以防止在遍历时重复搜索某个节点，这种做法叫做**状态记录或记忆化**。



#### 695、岛屿的最大面积

一般来说，深度优先搜索的题可以分为主函数和辅函数，主函数用来遍历所有的搜索位置，判断是否可以开始搜索，如果可以即在辅函数进行搜索。辅函数负责深度优先搜索的递归调用。当然，我们可以使用栈stack实现深度优先搜索。

递归

- 我们想知道网格中每个连通形状的面积，然后取最大值。

- 如果我们在一个土地上，以 4 个方向探索与之相连的每一个土地（以及与这些土地相连的土地），那么探索过的土地总数将是该连通形状的面积。

- 为了确保每个土地访问不超过一次，我们每次经过一块土地时，将这块土地的值置为 0。这样我们就不会多次访问同一土地。


栈

我们可以用栈来实现深度优先搜索算法。这种方法本质与方法一相同，唯一的区别是：

- 方法一通过函数的调用来表示接下来想要遍历哪些土地，让下一层函数来访问这些土地。而方法二把接下来想要遍历的土地放在栈里，然后在取出这些土地的时候访问它们。

- 访问每一片土地时，我们将对围绕它四个方向进行探索，找到还未访问的土地，加入到栈 stack 中；

- 另外，只要栈 stack 不为空，就说明我们还有土地待访问，那么就从栈中取出一个元素并访问。


```c++
// 递归
class Solution
{
	private:
		int dfs(vector<vector<int>>&grid, int cur_i, int cur_j)
		{
			if (cur_i < 0 || cur_j < 0 || cur_i == grid.size() || cur_j == grid[0].size()
				|| grid[cur_i][cur_j] != 1)
			{
				return 0;
			}

			grid[cur_i][cur_j] = 0;
			int di[4] = { 0, 0, 1, -1 };
			int dj[4] = { 1, -1, 0, 0 };
			int ans = 1;
			for (int index = 0; index != 4; ++index)
			{
				int next_i = cur_i + di[index], next_j = cur_j + dj[index];
				ans += dfs(grid, next_i, next_j);
			}
			return ans;
		}

	public:
		int maxAreaOfIsland(vector<vector<int>>&grid)
		{
			int ans = 0;
			for (int i = 0; i != grid.size(); ++i)
			{
				for (int j = 0; j != grid[0].size(); ++j)
				{
					ans = max(ans, dfs(grid, i, j));
				}
			}
			return ans;
		}
};

// 栈
class Solution
{
	public:
		int maxAreaOfIsland(vector<vector<int>>& grid)
		{
			int ans = 0;
			for (int i = 0; i < grid.size(); ++i)
			{
				for (int j = 0; j < grid[0].size(); ++j)
				{
					int cur = 0;
					stack<int> stacki;
					stack<int> stackj;
					stacki.push(i);
					stackj.push(j);
					while (!stacki.empty())
					{
						int cur_i = stacki.top(), cur_j = stackj.top();
						stacki.pop(); stackj.pop();
						if (cur_i < 0 || cur_j < 0 || cur_i == grid.size() || cur_j == grid[0].size()
							|| grid[cur_i][cur_j] != 1)
						{
							continue;
						}

						++cur;
						grid[cur_i][cur_j] = 0;
						int di[4] = { 0, 0, 1, -1 };
						int dj[4] = { 1, -1, 0, 0 };
						for (int index = 0; index != 4; ++index)
						{
							int next_i = cur_i + di[index], next_j = cur_j + dj[index];
							stacki.push(next_i); stackj.push(next_j);
						}
					}
					ans = max(ans, cur);
				}
			}
            return ans;
		}
};
```



#### 剑指567 朋友圈

```c++
int findCircleNum(vector<vector<int>>& friends)
{
	int n = friends.size(), count = 0;
	vector<bool> visited(n, false);
	for (int i = 0; i < n; ++i)
	{
		if (!visited[i])
		{
			dfs(friends, i, visited);
			++count;
		}
	}
	return count;
}

void dfs(vector<vector<int>>& friends, int i, vector<bool>& visited)
{
	visited[i] = true;
	for (int k = 0; k < friends.size(); ++k)
	{
		if (friends[i][k] == 1 && !visited[k])
		{
			dfs(friends, k, visited);
		}
	}
}
```



#### 417、太平洋大西洋水流问题

```c++
class Solution 
{
    public:
    vector<int> direction{ -1, 0, 1, 0, -1 };

    vector<vector<int>> pacificAtlantic(vector<vector<int>>& matirx)
    {
        if (matirx.empty() || matirx[0].empty())
        {
            return {};
        }

        vector<vector<int>> ans;
        int m = matirx.size(), n = matirx[0].size();
        vector<vector<bool>> can_reach_p(m, vector<bool>(n, false));
        vector<vector<bool>> can_reach_a(m, vector<bool>(n, false));
        for (int i = 0; i < m; ++i)
        {
            dfs(matirx, can_reach_p, i, 0);
            dfs(matirx, can_reach_a, i, n-1);
        }
        for (int i = 0; i < n; ++i)
        {
            dfs(matirx, can_reach_p, 0, i);
            dfs(matirx, can_reach_a, m-1, i);
        }
        for (int i = 0; i < m; i++)
        {
            for (int j = 0; j < n; ++j)
            {
                if (can_reach_p[i][j] && can_reach_a[i][j])
                {
                    ans.push_back(vector<int>{i, j});
                }
            }
        }
        return ans;
    }

    void dfs(const vector<vector<int>>& matirx, vector<vector<bool>>& can_reach,
        int r, int c)
    {
        if (can_reach[r][c]) return;

        can_reach[r][c] = true;
        int x, y;
        for (int i = 0; i < 4; ++i)
        {
            x = r + direction[i], y = c + direction[i + 1];
            if (x >= 0 && x < matirx.size() &&
                y >= 0 && y < matirx[0].size() &&
                matirx[r][c] <= matirx[x][y])
            {
                dfs(matirx, can_reach, x, y);
            }
        }
    }
};
```



### 回溯

​		回溯法是优先搜索的一种特殊情况，又称为试探法，常用于需要记录节点状态的深度优先搜索。通常来说，排列、组合、选择类问题使用回溯法比较方便。

​		在搜索到某一节点的时候，如果我们发现目前的节点（及其子节点）并不是需求目标时，我们回退到原来的节点继续搜索，**并且把在目前节点修改的状态还原**。这样的好处在于我们始终只对图的总状态进行修改，而非每次遍历时新建一个图来储存状态。

​		在具体写法上，与普通的深度优先搜索一样，都有【修改当前节点状态】——【递归子节点】的步骤，只是多了回溯的步骤，变成了【修改当前节点状态】——【递归子节点】——【回改当前节点】。

​		小诀窍：按引用传递状态，所有状态修改在递归完成后回改。

​		修改一般有两种情况：一种是修改最后一位输出，比如排列组合；一种是修改访问标记，比如矩阵里搜索字符串。



#### 46、全排列

给定一个**不含重复数字**的数组 `nums` ，返回其 **所有可能的全排列** 。你可以 **按任意顺序** 返回答案。

```c++
class Solution
 {
    private:
        vector<vector<int>> res;
        vector<int> path;

    public:
        vector<vector<int>> permute(vector<int>& nums)
        {
            int n = nums.size();
            vector<bool> used(n, false);
            
            backtrack(nums, used);
            return res;
        }

        void backtrack(vector<int>& nums, vector<bool>& used)
        {
            if (path.size() == nums.size())
            {
                res.push_back(path);
                return;
            }

            for (int i = 0; i < nums.size(); i++)
            {
                if (used[i] == true) continue;
                used[i] = true;
                path.push_back(nums[i]);
                backtrack(nums, used);
                path.pop_back();
                used[i] = false;
            }
        }
};
```

​	

#### 77、组合

```c++
class Solution 
{
    private:
        vector<vector<int>> res;
        vector<int> tmp;

    public:
        vector<vector<int>> combine(int n, int k) 
        {
            backTracking(n, k, 1);
            return res;
        }

        void backTracking(int n, int k, int cur)
        {
            if(tmp.size() == k)
            {
                res.push_back(tmp);
                return;
            }

            for(int i = cur; i <= n; i++)
            {
                tmp.push_back(i);
                backTracking(n, k, i+1);
                tmp.pop_back();
            }
        }
};
```

