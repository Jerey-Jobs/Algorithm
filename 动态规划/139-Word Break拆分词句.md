### 问题

给定一个字符串s和一组单词dict，判断s是否可以用空格分割成一个单词序列，使得单词序列中所有的单词都是dict中的单词（序列可以包含一个或多个单词）。
例如:
给定s=“leetcode”；
dict=["leet", "code"].
返回true，因为"leetcode"可以被分割成"leet code".

### 解法1

递归所有情况.深度遍历

这种解法能够求出正确结果,但是无法测试中规定时间内完成. 原因是因为递归会进行大量的重复计算.

``` java
import java.util.Arrays;
import java.util.Collections;
import java.util.HashSet;
import java.util.Set;

class WorkBreak {

    public static void main(String[] args) {
        Solution solution = new Solution();
        HashSet<String> set = new HashSet<String>(Arrays.asList("leet", "code", "hash", "d"));
        System.out.println(solution.wordBreak("leetdhashcode", set));
        System.out.println(solution.wordBreak("leet2code", set));

    }

    public static class Solution {
        public boolean wordBreak(String s, Set<String> dict) {
            return checkContains(0, s, dict);
        }

        public boolean checkContains(int start, String str, Set<String> dict) {
            if (start >= str.length()) {
                return true;
            }

            for (int i = start + 1; i <= str.length(); i++) {
                if (dict.contains(str.substring(start, i)) && checkContains(i, str, dict)) {
                    return true;
                }
            }
            return false;
        }
    }
}

```

### 解法2
直接用动态规划,f[i] 表示到此其是否可以划分为若干个单词,那么只要
f(i) = f(j) && f(j+1,i); 0 <= j < i;

``` java
public static class Solution3 {
    public static int count;
    public boolean wordBreak(String s, Set<String> dict) {
        int dp[] = new int[s.length() + 1];
        dp[0] = 1;
        for (int i = 0; i <= s.length(); i++) {
            if (dp[i] == 1) {
                for (int j = i; j <= s.length(); j++) {
                    if (dict.contains(s.substring(i, j))) {
                        dp[j] = 1;
                    }
                }
            }
        }
        return dp[s.length()] == 1;
    }
```
