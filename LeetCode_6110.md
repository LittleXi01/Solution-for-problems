# 6110.网格图中递增路径的数目
## [题目链接](https://leetcode.cn/problems/number-of-increasing-paths-in-a-grid/)<来源：力扣（LeetCode）>
### 题目描述
给你一个 m x n 的整数网格图 grid ，你可以从一个格子移动到 4 个方向相邻的任意一个格子。

请你返回在网格图中从 任意 格子出发，达到 任意 格子，且路径中的数字是 严格递增 的路径数目。由于答案可能会很大，请将结果对 109 + 7 取余 后返回。

如果两条路径中访问过的格子不是完全相同的，那么它们视为两条不同的路径。
#### 示例
输入：grid = [[1,1],[3,4]]  
输出：8  
解释：严格递增路径包括：
- 长度为 1 的路径：[1]，[1]，[3]，[4] 。
- 长度为 2 的路径：[1 -> 3]，[1 -> 4]，[3 -> 4] 。
- 长度为 3 的路径：[1 -> 3 -> 4] 。
路径数目为 4 + 3 + 1 = 8 。
#### 提示：
m == grid.length  
n == grid[i].length  
1 <= m, n <= 1000  
1 <= m * n <= 1E5  
1 <= grid[i][j] <= 1E5

### 分析：
  本题要在一个二维数组中搜索严格单调递增的序列的数目，可以考虑dfs算法，对二维数组中的每个数字进行遍历，然后整合答案。
但是这种方法采用C++实现的话很容易超时（python不会），所以要考虑更加优化的算法，不难想到可以利用记忆化搜索实现，对每个
已经进行了深度优先搜索的格子进行记录，直接复用已经被记录的格子的方案数目，可以大大节省遍历的时间。

#### 基于记忆化搜索的dfs函数

    void dfs(vector<vector<long long>>& vec,vector<vector<int>>& grid, int x, int y, int ans,int predir)
        {
            int m = grid.size();
            int n = grid[0].size();
            int dix[4] = { 0,1,0,-1 };
            int diy[4] = { 1,0,-1,0 };
            int mod = pow(10, 9) + 7;
            for (int i = 0; i < 4; i++)
            {
                if (ifon(x+dix[i], y + diy[i], m, n) && grid[x + dix[i]][y + diy[i]] > grid[x][y])
                {

                    if (vec[x + dix[i]][y + diy[i]] == 1)
                    {
                        vec[x][y] %= mod;
                        dfs(vec,grid, x + dix[i], y + diy[i], ans, i);
                        vec[x][y] += vec[x + dix[i]][y + diy[i]];                   
                    }
                    else
                        vec[x][y]+= vec[x + dix[i]][y + diy[i]];  
                    vec[x][y] %= mod;
                }
            }
        }
        
#### 完整代码：

    class Solution {
    public:
        bool ifon(int x, int y, int m, int n)
        {
            return x >= 0 && x < m&& y >= 0 && y < n;
        }
        void dfs(vector<vector<long long>>& vec,vector<vector<int>>& grid, int x, int y, int ans,int predir)
        {
            int m = grid.size();
            int n = grid[0].size();
            int dix[4] = { 0,1,0,-1 };
            int diy[4] = { 1,0,-1,0 };
            int mod = pow(10, 9) + 7;
            for (int i = 0; i < 4; i++)
            {
                if (ifon(x+dix[i], y + diy[i], m, n) && grid[x + dix[i]][y + diy[i]] > grid[x][y])
                {

                    if (vec[x + dix[i]][y + diy[i]] == 1)
                    {
                        vec[x][y] %= mod;
                        dfs(vec,grid, x + dix[i], y + diy[i], ans, i);
                        vec[x][y] += vec[x + dix[i]][y + diy[i]];                   
                    }
                    else
                        vec[x][y]+= vec[x + dix[i]][y + diy[i]];  
                    vec[x][y] %= mod;
                }
            }
        }

        int countPaths(vector<vector<int>>& grid) {
            int m = grid.size();
            int n = grid[0].size();
            int mod = 1E9 + 7; 
            vector<long long> v(n, 1);
            vector<vector<long long>> vec(m,v);
            long long ans = 0;
            for (int i = 0; i < m; i++)
            {
                for (int j = 0; j < n; j++)
                {
                    if (vec[i][j] == 1)
                    dfs(vec,grid, i, j, ans, 0);
                }

            }
            for (int i = 0; i < m; i++)
            {
                for (int j = 0; j < n; j++)
                {              
                    ans += vec[i][j];
                    ans%= mod;
                }
            }
            return ans;
        }
    };

##### @author:LittleXi
##### [bilibili主页](https://space.bilibili.com/524432272?spm_id_from=333.337.0.0)
##### [Leetcode主页](https://leetcode.cn/u/stupefied-7umierebon/)
##### [github主页](https://github.com/LittleXi01)


