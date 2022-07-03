# 6109. 知道秘密的人数<来源：力扣（LeetCode）>
## [题目链接](https://leetcode.cn/problems/number-of-people-aware-of-a-secret/)

### 题目描述

在第 1 天，有一个人发现了一个秘密。

给你一个整数 delay ，表示每个人会在发现秘密后的 delay 天之后，每天 给一个新的人 分享 秘密。同时给你一个整数 forget ，表示每个人在发现秘密 forget 天之后会 忘记 这个秘密。一个人 不能 在忘记秘密那一天及之后的日子里分享秘密。

给你一个整数 n ，请你返回在第 n 天结束时，知道秘密的人数。由于答案可能会很大，请你将结果对 1E9 + 7 取余 后返回。

### 示例：
输入：n = 6, delay = 2, forget = 4  
输出：5  
解释：  
第 1 天：假设第一个人叫 A 。（一个人知道秘密）  
第 2 天：A 是唯一一个知道秘密的人。（一个人知道秘密）  
第 3 天：A 把秘密分享给 B 。（两个人知道秘密）  
第 4 天：A 把秘密分享给一个新的人 C 。（三个人知道秘密）  
第 5 天：A 忘记了秘密，B 把秘密分享给一个新的人 D 。（三个人知道秘密）  
第 6 天：B 把秘密分享给 E，C 把秘密分享给 F 。（五个人知道秘密）  

### 分析：
    这道题目每一天新增的人数是[delay,forget)区间的人数总和，所以可以考虑应用动态规划解题。
    可以尝试用队列deque<int> deq<因为方便索引，所以就没用queue>;存储每个状态的人数。
    dp[n]存储每天新增的人数，即dp[n]=n-1天对应时的∑([deq[delay-1],deq[forget]))
    然后及时更新deq，即deq.pop_back(),deq.push_front(dp[n])。
    最后返回∑dp即可
    时间复杂度O(n*(forget-delay));
    空间复杂度O(forget);

### 完整代码；
    class Solution {
    public:
        int peopleAwareOfSecret(int n, int delay, int forget) {
            deque<int> q(forget-1,0);
            int mod = 1E9 + 7;
            long ans = 0;
            long temp = 0;
            q.push_front(1);
            for (int i = 2; i <= n; i++)
            {
                /*temp += q[delay - 1];
                temp -= q.back();*/
                temp = 0;
                for (int i = delay-1; i < forget-1; i++)
                {
                    temp += q[i];
                }
                q.pop_back();
                temp %= mod;
                q.push_front(temp);
            }
            for (int i = 0; i < forget; i++)
            {
                ans += q[i];
                ans %= mod;
            }
            return ans;
        }
    };

##### @author:LittleXi
##### [bilibili主页](https://space.bilibili.com/524432272?spm_id_from=333.337.0.0)
##### [Leetcode主页](https://leetcode.cn/u/stupefied-7umierebon/)
##### [github主页](https://github.com/LittleXi01)

