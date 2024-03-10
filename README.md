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

![alt text](images/1-4-0.png)

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

### 1.5 [打家劫舍](https://leetcode.cn/problems/house-robber/description/?envType=study-plan-v2&envId=dynamic-programming)

> 对于最后一家 下标 n只有两种结果，抢还是不抢，不抢是s[n-1] + 0 抢了是s[n-2] + nums[n]，比较二者即可

```Java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            //s1
            return nums[0];
        }
        if (n == 2) {
            //s2
            return Math.max(nums[0], nums[1]);
        }
        // sn = Math.max(s[n-1],s[n-2]+nums[n-1])
        int si=0,pre=nums[0],curr=Math.max(nums[0], nums[1]);
        for (int i = 3; i <= n; i++) {
            si = Math.max(curr,pre+nums[i-1]);
            pre = curr;
            curr = si;
        }
        return si;
    }
}
```

### 1.6 [删除并获得点数](https://leetcode.cn/problems/delete-and-earn/description/?envType=study-plan-v2&envId=dynamic-programming)

> 每个数字不管出现几次，只要拿了就应该全部拿光，这样也就可以将出现的数字去重排序，中间缺位的用0 补齐 比如[2,2,2,5,4] 可以去重排序补齐为[2,0,4,5],进而变形为[6,0,4,5]就可以按照1.5 打家劫舍中的情况进行处理，相邻的不能都取。

```Java
class Solution {
    public int deleteAndEarn(int[] nums) {
        Map<Integer, Integer> map = new TreeMap<>();

        for (int num : nums) {
            map.compute(num, new BiFunction<Integer, Integer, Integer>() {
                @Override
                public Integer apply(Integer t, Integer u) {
                    u = u == null ? 0 : u;
                    return t + u;
                }
            });
        }
        List<Integer> keyList = new ArrayList<>(map.keySet());
        int min = keyList.get(0);
        int len = keyList.get(keyList.size() - 1) - keyList.get(0);
        int[] newNums = new int[len + 1];
        for (int i = 0; i <=len ; i++) {
            newNums[i] = map.containsKey(i+min) ? map.get(i+min) : 0;
        }

        //System.out.println(map);

        if (newNums.length == 1) {
            return newNums[0];
        }
        if (newNums.length == 2) {
            return Math.max(newNums[0], newNums[1]);
        }
        int pre = newNums[0], curr = Math.max(newNums[0], newNums[1]);
        int sn = 0;
        for (int n = 3; n <= newNums.length; n++) {
            sn = Math.max(pre + newNums[n - 1], curr);
            pre = curr;
            curr = sn;
        }
        //System.out.println(sn);
        return sn;
    }
}
```