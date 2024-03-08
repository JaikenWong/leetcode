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