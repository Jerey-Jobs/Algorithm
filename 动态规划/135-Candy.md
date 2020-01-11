### 问题
There are N children standing in a line. Each child is assigned a rating value.

You are giving candies to these children subjected to the following requirements:

Each child must have at least one candy.
Children with a higher rating get more candies than their neighbors.
What is the minimum candies you must give?



题目：
N个孩子站成一排，给每个人设定一个权重（已知）。按照如下的规则分配糖果： (1)每个孩子至少分得一颗糖果 （2）权重较高的孩子，会比他的邻居获得更多的糖果。

问：总共最少需要多少颗糖果？请分析算法思路，以及算法的时间，空间复杂度是多少。

### 思路
孩子的权重数组: c[N]

从左往右扫,再从右往左扫

``` java
import java.util.Arrays;

class Candy {
    public static void main(String[] args) {
        Solution solution = new Solution();
        solution.candy(new int[]{4,2,3,4,1});
    }

    public static class Solution {
        public int candy(int[] ratings) {
            int dp[] = new int[ratings.length];
            for (int i = 0; i < dp.length; i++) {
                dp[i] = 1;
            }

            if (ratings.length  == 0) return 0;
            if (ratings.length  == 1) return 1;

            for (int i = 1; i < dp.length; i++) {
                if (ratings[i] > ratings[i - 1]) {
                    dp[i] = dp[i - 1] + 1;
                }
            }

            for (int i = dp.length - 1 ; i > 0; i--) {
                if (ratings[i -1 ] > ratings[i] && dp[i - 1] <= dp[i]) {
                    dp[i - 1] = dp[i] + 1;
                }
            }
            int sum = 0;
            for (int i = 0; i < dp.length; i++) {
                sum += dp[i];
            }
            System.out.println(Arrays.toString(dp));
            return sum;
        }
    }
}

```
