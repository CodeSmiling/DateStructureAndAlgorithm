# Set集合

### 目录

* [数组中的重复数字](#数组中的重复数字)


---
### 数组中的重复数字
Q:在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，
也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3

A:判断重复元素可以使用set集合
```java
 public int findRepeatNumber(int[] nums) {
       int count=0;
        Set<Integer> hashSet=new HashSet<>();
        for(int num:nums){
            hashSet.add(num);
            count++;
            if(hashSet.size()!=count){
                return num;
            }
        }
        return 0;
    }
```
