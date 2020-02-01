### 问题
* 给出一个字符串s，分割s使得分割出的每一个子串都是回文串
* 计算将字符串s分割成回文分割结果的最小切割数
* 例如:给定字符串s="aab",
* 返回1，因为回文分割结果["aa","b"]是切割一次生成的。

### 思路
1. 先搞定判断一个字符串某段是回文的方法,特别简单
2. 如果字符串是回文,那么dp[i] = 0, 如果不是,先认为其分割次数应该是dp[i] = i-1 次  
3. 然而dp[i]不大可能是i - 1次, 因为0-i中可能是有有回文的字符串的
因此,我们从一开始就进行如下的操作
dp[i] = 0; if 0-i为回文
dp[i] = min{dp[j- 1] + 1,dp[i]}; if 0-i 不为回文; 但是 dp[j-i]为回文
dp[i] = min{dp[j - 1] + 1 +i -j,dp[i]}; if 0-i 不为回文; 且 dp[j-i]也不为回文


### code
/**
* 给出一个字符串s，分割s使得分割出的每一个子串都是回文串
* 计算将字符串s分割成回文分割结果的最小切割数
* 例如:给定字符串s="aab",
* 返回1，因为回文分割结果["aa","b"]是切割一次生成的。
 */
class PalindromePartitionII {

    public static void main(String[] args) {
        System.out.println(new Solution().minCut("dde"));
    }


    public static class Solution {
        public int minCut(String s) {
            int dp[] = new int[s.length()];
            int len = s.length();
            for (int i = 0; i < len; i++) {
                if (isPartition(s, 0, i)) {
                    dp[i] = 0;
                } else {
                    dp[i] = i;
                    for (int j = 1; j <= i; j++) {
                        if (isPartition(s, j, i)) {
                            dp[i] = Math.min(dp[i], dp[j - 1] + 1);
                        } else {
                            dp[i] = Math.min(dp[i], dp[j - 1] + i - j + 1);
                        }
                    }
                }
            }
            return dp[len - 1];
        }


        public boolean isPartition(String s, int left, int right) {
            if (left == right) {
                return true;
            }

            while (left < right) {
                if (s.charAt(left) != s.charAt(right)) {
                    return false;
                }
                left++;
                right--;
            }
            return true;
        }
    }
}
