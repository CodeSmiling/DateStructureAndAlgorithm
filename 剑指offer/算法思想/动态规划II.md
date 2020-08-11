## 动态规划II

### 剪绳子I 
- 题目链接：https://leetcode-cn.com/problems/jian-sheng-zi-lcof/
- 思考
  - 状态：索引不断变化，一维数组
  - 函数：dp[i]表示数字i的最大乘积
  - 选择：选择i,dp[n - i]*dp[i];
base case:dp[2] = 1;dp[3] = 2;
```JAVA
class Solution {
    public int cuttingRope(int n) {
        if(n == 2)
        return 1;
        if(n == 3)
        return 2;
        int [] dp = new int[n + 1];
        dp[1] = 1;
        dp[2] = 2;
        dp[3] = 3;
        for(int i = 4; i <= n; i++){
            for(int j = 1; j < i; j++){
                dp[i] = Math.max(dp[i], dp[i - j]*dp[j]);
            }
        }
        return dp[n];
    }
}
class Solution{
    public int cuttingRope(int n) {
        if(n <= 3) return n - 1;
        int a = n/3;
        int b = n%3;
        int ans = 0;
        if(b == 0){
            ans = (int)Math.pow(3, a);
        }
        if(b == 1){
            ans = (int)Math.pow(3, a - 1)*4;
        }
        if(b == 2){
            ans = (int)Math.pow(3, a)*2;
        }
        return ans;
    }
}
```
### 剪绳子II
- 题目链接：https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/
- tip:大数求余的技巧
```JAVA
class Solution{
    public int cuttingRope(int n){
        if(n <= 3) return n - 1;
        int b = n % 3;
        int p = 1000000007;
        int rem = 1;
        int x = 3;
        //大数求余
        for(int i = n/3 - 1; i > 0; i--){
            rem = (rem * x) % p;
            x = (x * x) % p; 
        }
        if(b == 0){
            return (int)(rem * 3 % p);
        }
        if(b == 1){
            return (int)(rem * 4 % p);
        }
        return (int)(rem * 6 % p);
    }     
}
```
### 连续子数组的最大和
- 题目链接：https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/
- 思路
  - 状态：索引不断变化一维数组
  - 函数：dp[i]表示到索引i的连续数组最大值
  - 选择：如果dp[i - 1]大于0，dp[i] = dp[i - 1] + nums[i];否则dp[i] = nums[i];
```JAVA
class Solution {
    public int maxSubArray(int[] nums) {
        int length = nums.length;
        if(length == 0) return 0;
        int[] dp = new int[length];
        dp[0] = nums[0];
        int res = nums[0];
        //遍历这个数组
        for(int i = 1; i < length; i++){
            if(dp[i - 1] > 0){
                dp[i] = dp[i - 1] + nums[i];
            }
            else{
              dp[i] = nums[i];  
            }
            res = Math.max(res,dp[i]);
        }
        return res;
    }
}
```
- 进阶写法
```JAVA
class Solution {
    public int maxSubArray(int[] nums) {
        int length = nums.length;
        if(length == 0) return 0;
        int res = nums[0];
        int dp_i = Integer.MIN_VALUE;
        //遍历这个数组
        for(int i = 0; i < length; i++){
            if(dp_i > 0){
                dp_i = dp_i + nums[i];
            }
            else{
              dp_i = nums[i];  
            }
            res = Math.max(res,dp_i);
        }
        return res;
    }
}
```
### 礼物的最大价值
- 思路
  - 状态：索引i,j 使用二位数组。
  - 函数：dp[i][j]表明在i,j索引处的最大值
  - 选择：索引i，j变化从上或者是从左的最大值加上当前值dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
          递推公式可以利用已有的数组 grid[i][j] = Math.max(grid[i - 1][j], grid[i][j - 1]) + grid[i][j]
  - base case
    - 当i == 0 || j == 0需要单独使用
```JAVA
class Solution{
    public int maxValue(int[][] grid){
        int row = grid.length;
        int col = grid[0].length;
        if(row == 0) return 0;
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                if(i == 0 && j == 0) dp[i][j] = grid[i][j];
                else if(i == 0) dp[i][j] = dp[i][j - 1] + grid[i][j];
                else if(j == 0) dp[i][j] = dp[i - 1][j] + grid[i][j];
                else dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }
        return dp[row - 1][col - 1];
    }
```
- 进阶写法
```java
class Solution{
    public int maxValue(int[][] grid){
        int row = grid.length;
        int col = grid[0].length;
        if(row == 0) return 0;
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                if(i == 0 && j == 0) continue;
                else if(i == 0) grid[i][j] = grid[i][j - 1] + grid[i][j];
                else if(j == 0) grid[i][j] = grid[i - 1][j] + grid[i][j];
                else grid[i][j] = Math.max(grid[i - 1][j], grid[i][j - 1]) + grid[i][j];
            }
        }
        return grid[row - 1][col - 1];
    }
```
### 最长不含重复的字符的子字符串
- 题目链接：https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/
- 思路
  - 状态：索引i不断变化，一维数组
  - 函数：dp[i]表示以i结尾最长的无重复字符串长度
  - 选择：当s[i] == s[j]的时候，表明有重复dp[j - 1] >= j - i; dp[i] = j - i。否则为dp[j] = dp[j - 1] + 1;
```java
class Solution{
    public int lengthOfLongestSubstring(String s) {
        Map<Character,Integer> map = new HashMap<>();
        int res = 0;
        int dp = 0;
        for(int j = 0; j < s.length(); j++){
            int i = map.getOrDefault(s.charAt(j), -1);
            map.put(s.charAt(j), j);
            if(dp < j - i ){
                dp = dp + 1;
            }else{
                dp = j - i;
            }
            res = Math.max(res, dp);
        }
        return res;
    }
}
```
### 丑数
- 题目链接：https://leetcode-cn.com/problems/chou-shu-lcof/
- 思路
  - 状态：索引不断变化
  - 函数：dp[i]第i个丑数
  - 选择：索引变化是需要比较质因子为2，3，5的数字的最小值。
  其中质因子为2的丑数是数组的值依次增加然后乘以2。
  其中质因子为3的丑数是数组的值依次增加然后乘以3。
  其中质因子为5的丑数是数组的值依次增加然后乘以5。
  所以需要三个索引依次增长然后选择最小值
  - base case
    - dp[1] = 1;
```java
class Solution {
    public int nthUglyNumber(int n) {
        int[]dp = new int[n + 2];
        dp[1] = 1;
        int a = 1, b = 1, c = 1;
        int res = 1;
        for(int i = 2; i <= n; i++){
            int n1 = dp[a] * 2;
            int n2 = dp[b] * 3;
            int n3 = dp[c] * 5;
            dp[i] = Math.min(Math.min(n1, n2), n3);
            if(dp[i] == n1) a++;
            if(dp[i] == n2) b++;
            if(dp[i] == n3) c++;
        }
        return dp[n];
    }
}
```
### 正则表达式的匹配
- 题目链接:https://leetcode-cn.com/problems/regular-expression-matching/
- 思路:动态规划
  - 如果p[j] != '*'
    - if(s[i] == s[j] || s[j] == '.') dp[i][j] = dp[i - 1][j - 1]
    - dp[i][j] = false;
  - 如果dp[j] == '*'
    - 选择不匹配
      dp[i][j] |= dp[i][j - 2]; 
    - 选择匹配
      dp[i][j] |= dp[i - 1][j];   
    
```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length();
		int n = p.length();
		boolean[][] dp = new boolean[m + 1][n + 1]; 
		for(int i = 0; i <= m; i++){
			for(int j = 0; j <= n; j++){
				if(j == 0){
					dp[i][j] = i==0;
				}
				else{
					if(p.charAt(j - 1) != '*'){
						if(i > 0 && (s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '.')){
							dp[i][j] = dp[i - 1][j - 1];
						}
					}
					else{
						//不看
						if(j >= 2){
							dp[i][j] |= dp[i][j - 2];
						}
						//看
						if(i >= 1 && j >= 2 && (s.charAt(i - 1) == p.charAt(j - 2) || p.charAt(j - 2) == '.')){
							dp[i][j] |= dp[i - 1][j];
						}
					}
				}
			}
		}
		return dp[m][n];
    }
}
```
