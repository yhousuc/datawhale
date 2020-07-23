# 每日一题：LeetCode 64. 最小路径和

给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。

示例:

```
输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。
```

**解析：**

思路：起始点：grid[0]\[0] ， 从该点开始每次只能 **向下** 或者 **向右** 移动一步

​			目标：设置一个dp[]\[] 数组， 数组维度与 grid 一致，用来存储从起始点到该点的最短距离

   		 方法：动态规划

1）对于第一行的每个网格，只能从左边的节点 **向右** 移动一步获得

```C++
for(int i = 1; i < cols; ++i)
{
    dp[0][i] = dp[0][i-1] + grid[0][i];
}
```

2）对于第一列的每个网格，只能从上边的节点 **向下** 移动一步获得

所以：

```C++
for(int j = 1; j < rows; ++j)
{
	dp[j][0] = dp[j-1][0] + grid[j][0];
}
```



3) 对于从 dp[1]\[1] 开始，其值取 左网格 左移动一格， 上网格 下移动一格 两者的最小值，作为该值的更新

所以~代码如下：

```C++
class Solution{
public:
    int minPathSum(vector<vector<int>>& grid)
    {
        if(grid.size()==0 || grid[0].size()==0) return 0;
        int rows = grid.size();
        int cols = grid[0].size();
        //声明一个最小的距离矩阵
        vector<vector<int>> dp(rows, vector<int>(cols));
        dp[0][0] = grid[0][0];//初始化起点
        for(int i = 1; i < cols; ++i)
        {
            dp[0][i] = dp[0][i-1] + grid[0][i];
        }
        for(int j = 1; j < rows; ++j)
        {
            dp[j][0] = dp[j-1][0] + grid[j][0];
        }
        //这里既可以每一行的赋值，又可以每一列的赋值，这里选择每一行从左到右依次赋值
        for(int i = 1; i < rows; ++i)
        {
            for(int j = 1; j < cols; ++j)
            {
                dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
            }
        }
        return dp[rows-1][cols-1];
    }
};
```

