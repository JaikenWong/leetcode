[TOC]

# Leetcode

## 1. 动态规划基础版

### 1.1 [爬楼梯](https://leetcode.cn/problems/climbing-stairs/?envType=study-plan-v2&envId=dynamic-programming)

> 一阶楼梯只有一种爬法，二阶楼梯有两种爬法 这可以作为初始的已知值，并且f(n) = f(n-1) +f(n-2)
```java
class Solution {
    public int climbStairs(int n) {
        int s1 = 1,s2 = 2;
        if(n == 1){
            return s1;
        }else if(n == 2){
            return s2;
        }
        int sn = 0;
        for(int i=2;i<n;i++){
            sn = s1+ s2;
            s1 = s2;
            s2 = sn;
        }
        return sn;
    }
}
```

### 1.2 [斐波那契数列](https://leetcode.cn/problems/fibonacci-number/description/?envType=study-plan-v2&envId=dynamic-programming)

>同上，相同的状态转移方程，改变下初始值就行

```java
class Solution {
    public int fib(int n) {
        int s0 = 0,s1 = 1;
        if(n == 0){
            return s0;
        }else if(n == 1){
            return s1;
        }
        int sn = 0;
        for(int i=2;i<=n;i++){
            sn = s0+ s1;
            s0 = s1;
            s1 = sn;
        }
        return sn;
    }
}
```

### 1.3 [第 N 个泰波那契数](https://leetcode.cn/problems/n-th-tribonacci-number/description/?envType=study-plan-v2&envId=dynamic-programming)

> 同样性质的给出初始值 T0 = 0, T1 = 1, T2 = 1, 且在 n >= 0 的条件下 Tn+3 = Tn + Tn+1 + Tn+2

```Java
class Solution {
    public int tribonacci(int n) {
        int s0 = 0,s1 = 1,s2=1;
        if(n == 0){
            return s0;
        }else if(n == 1 || n ==2){
            return s1;
        }
        int sn = 0;
        for(int i=3;i<=n;i++){
            sn = s0+ s1+s2;
            s0 = s1;
            s1 = s2;
            s2 = sn;
        }
        return sn;
    }
}

```

### 1.4 [使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/?envType=study-plan-v2&envId=dynamic-programming)

> 1. 假设有没有楼梯 直接走过 dp[0] = 0
> 2. 假设有一层楼梯，可以直接以一次上两层楼梯的方式走过，消费 dp[1] =0 也就是走过一层的最小消费；
假设有二层楼梯，要么先走第一层，迈过第二层，要么迈过第一层，走第二层然后过去 dp[2] = Min(cost[0],cost[1]) ，这是到走过二层的最小消费；
> 3. 假设有三层楼梯，想要走过去，要么迈过第三层，要么经过第三层，cost[1]是踩上第二层，迈过了第三层的消费，cost[2]是踩上了第三层的消费，分别在加上到达这一层消费前的最小消费，得到两种方式分别的最小消费，再比较出小的就是最终最小花费。dp[3] = Min(dp[2]+ cost[2],dp[1]+ cost[1]); 这是走过三层的最小消费。
> 4. 假设有四层楼梯，想要走过，要么从第二层经过第四层，要么从第三层迈过第四层，dp[4] = Min(dp[3]+ cost[3],dp[2]+ cost[2])
> 5. 同理 对任意层楼梯n时，要么经过第n层，要么迈过第n层, 经过时最后一次消费是cost[n-1],迈过时最后一次消费是cost[n-2],加上之前的最小值 dp[n] = Min(dp[n-1]+ cost[n-1],dp[n-2]+cost[n-2])


```Java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length;
        int[] dp = new int[n+1];
        dp[0] = 0;
        dp[1] = 0;
        if(n == 2){
            return Math.min(cost[0],cost[1]);
        }
        for(int i= 2;i<=n;i++){
            dp[i] = Math.min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2]);
        }
        return dp[n];
    }
}
```