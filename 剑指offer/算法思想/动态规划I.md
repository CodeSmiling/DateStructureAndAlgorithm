## 动态规划I
* [股票最大利润I](#股票最大利润I)
* [股票最大利润II](#股票最大利润II)
* [股票最大利润III](#股票最大利润问题III)
* [股票最大利润IV](#股票最大利润问题IV)
### 股票问题
- 递归函数 
    - dp[i][k][1]:表明第i天最多进行k次交易手中持有股票
    - dp[i][k][0]:表明第i天最多进行k次交易手中不持有股票
- 状态转函数
    dp[i][k][0] = Math.max(dp[i - 1][k][0], dp[i - 1][k][1] + prices[i]);
                                         昨天没有持有股票            今日出售股票
    dp[i][k][1] = Math.max(dp[i - 1][k][1], dp[i - 1][k - 1][0] - prices[i]); 
                                         昨天保持股票                今日购买股票(此时可交易次数要减少)
### 股票最大利润I
- 题目链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/
由题意可知k为1，dp[0][0][i] = 0;
```java
class Solution {
    public int maxProfit(int[] prices) {
    - 基础写法
        int length = prices.length;
        if(length == 0) return 0;
        int[][] dp = new int[length][2];
        for(int i = 0; i < length; i++){
            //base case
            if(i == 0){
                dp[0][0] = 0;
                dp[0][1] = -prices[0];
                continue;
            }
            //dp[0][0][i] = 0,表明交易0次手中持有股票的利润为0; 
            //递推式中当前状态至于上一个状态相关
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1],  - prices[i]);
        }
        return dp[length - 1][0];
    }
```
  - 进阶写法
```java
     public int maxProfit(int[] prices) {
        int length = prices.length;
        if(length == 0) return 0;
        //由题意可知k为1
        int dp_i_0 = 0;
        int dp_i_1 = -prices[0];
        for(int i = 1; i < length; i++){
            dp_i_0 = Math.max(dp_i_0, dp_i_1 + prices[i]);
            dp_i_1 = Math.max(dp_i_1,  - prices[i]);
        }
        return dp_i_0;
    }
}
```
### 股票最大利润II
- 题目链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/
k = infinite,此时k-1与k相同
```java
class Solution {
    - 常规写法
    public int maxProfit(int[] prices) {
       int length = prices.length;
       if(length == 0) return 0;
       int[][] dp = new int[length][2];

       for(int i = 0; i < length; i++){
            if(i == 0){
                //base case
                dp[0][0] = 0;
                dp[0][1] = -prices[0];
                continue;
            }
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
       }
       return dp[length - 1][0];
    }
```
 - 进阶写法
```java
    public int maxProfit(int[] prices) {
       int length = prices.length;
       if(length == 0) return 0;
       //base case
       int dp_i_0 = 0;
       int dp_i_1 = -prices[0];
       for(int i = 0; i < length; i++){
            int temp = dp_i_0;
            dp_i_0 = Math.max(dp_i_0, dp_i_1 + prices[i]);
            dp_i_1 = Math.max(dp_i_1, temp - prices[i]);
       }
       return dp_i_0;
    }
}
```
### 股票最大利润问题III
- 题目链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/
```java
class Solution {
    public int maxProfit(int[] prices) {
       int max_k = 2;
       int length = prices.length;
       int[][][] dp = new int[length][max_k + 1][2];
       for(int i = 0; i < length; i++){
            for(int j = max_k; j >= 1; j--){
                if(i == 0){
                    //base case
                    dp[0][j][0] = 0;
                    dp[0][j][1] = -prices[0];
                    continue;
                }
                dp[i][j][0] = Math.max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i]);
                dp[i][j][1] = Math.max(dp[i - 1][j][1], dp[i - 1][j][0] - prices[i]);
            }  
       }
       return dp[length - 1][max_k][0];
    }
}
```

### 股票最大利润问题IV
- 题目链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/
```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        int length = prices.length;
        if(length == 0) return 0;
        if(k < length / 2){
            return maxProfit(prices);
        }
        int[][][] dp = new int[length][k + 1][2];
        for(int i = 0; i < length; i++){
            for(int j = k; j >= 1; j--){
                if(i == 0){
                    dp[0][j][0] = 0;
                    dp[0][j][1] = -prices[0];
                    continue;
                }
                dp[i][j][0] = Math.max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i]);
                dp[i][j][1] = Math.max(dp[i - 1][j][1], dp[i - 1][j][0] - prices[i]);
            }
        }
        return dp[length - 1][k][0];
    }
    private int maxProfit(int[] prices){
        int length = prices.length;
        if(length == 0) return 0;
        //base case
        int dp_i_0 = 0;
        int dp_i_1 = -prices[0];
        for(int i = 0; i < length; i++){
            int temp = dp_i_0;
            dp_i_0 = Math.max(dp_i_0, dp_i_1 + prices[i]);
            dp[i][1] = Math.max(dp_i_1, temp - prices[i]);
        }
        return dp[length - 1][0];
    }
}
```
