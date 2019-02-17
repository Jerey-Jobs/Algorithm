### 编辑距离 (Levenshtein Distance算法)
字符串的编辑距离，又称为Levenshtein距离，由俄罗斯的数学家Vladimir Levenshtein在1965年提出。是指利用字符操作，把字符串A转换成字符串B所需要的最少操作数。其中，字符操作包括：

删除一个字符     a) Insert a character <br>
插入一个字符     b) Delete a character<br>
修改一个字符     c) Replace a character<br>
例如对于字符串"if"和"iff"，可以通过插入一个'f'或者删除一个'f'来达到目的。

  一般来说，两个字符串的编辑距离越小，则它们越相似。如果两个字符串相等，则它们的编辑距离（为了方便，本文后续出现的“距离”，如果没有特别说明，则默认为“编辑距离”）为0（不需要任何操作）。不难分析出，两个字符串的编辑距离肯定不超过它们的最大长度（可以通过先把短串的每一位都修改成长串对应位置的字符，然后插入长串中的剩下字符）。

### 问题描述
给定两个字符串A和B，求字符串A至少经过多少步字符操作变成字符串B。

### 问题解决

1. 当其中某个字符串长度为0的时候,编辑距离就是另一个字符串的长度. (我们可以理解为, 对长度为0的字符串一直插入字符变成另一个字符串)
2. 当字符串不等的时候, 我们总是习惯性的从字串开头开始看.

   那么A[0] = B[0];的时候, 那么此时编辑距离依旧是0, 我们可以直接去除字符串的第一个字符了. 因为此时A与B的编辑距离应该是等于A[1]..A[A.length-1], B[1]..B[B.length-1]两者的编辑距离的.

   如果A[0] != B[0], 那么此时我们要考虑的很多了, A[0] 会不会与B[1]相等, 这样只要添加一个字符就可以了. B[0] 会不会与A[1]相等, 或者A[1]与B[1]也不相等. 这样

   若我们从后面往前看,ij代表a,b 的长度,我们让求编辑距离的方法为f

   当 a[i] = a [j] 时候,f(i, j) = f(i-1, j-1);<br>
   a[i] != a [j] 时候,f(i, j) = f(i-1, j-1) + 1; 或者是 f(i, j-1) +1 或者是f(i-1, j) + 1;

   那么此时动态转移方程为<br>
``` java
   f(i,j) = max(i,j)  if i与j其中一个为0<br>
   f(i,j) = f(i-1,j-1) if a[i]=a[j]
   f(i,j) = min (f(i-1,j-1) + 1,
                f(i, j-1) + 1,
                f(i-1, j) + 1);
```

这是一个动态规划问题.使用公式我们可以很快写出递归方法
``` java
public static int getEditDistanceByRecursion(String a, String b, int aIndex, int bIntex) {
    if (Math.min(aIndex, bIntex) == 0) {
        return Math.max(aIndex, bIntex);
    }
    if (a.charAt(aIndex) == b.charAt(bIntex)) {
        return getEditDistanceByRecursion(a, b, aIndex - 1, bIntex - 1);
    }

    return Math.min(getEditDistanceByRecursion(a, b, aIndex - 1, bIntex - 1) + 1,
            Math.min(getEditDistanceByRecursion(a, b, aIndex, bIntex - 1) + 1,
                    getEditDistanceByRecursion(a, b, aIndex - 1, bIntex) + 1));
}
```

但是递归的最大缺点为重复计算. 多次计算同一个结果. 我们需要一个表来存储重复计算的结果.

代码如下

``` java
public static int getEditDistance(String origin, String target) {

    if (TextUtils.isEmpty(origin) && TextUtils.isEmpty(target)) {
        return 0;
    }

    if (TextUtils.isEmpty(origin)) {
        return target.length();
    }

    if (TextUtils.isEmpty(target)) {
        return origin.length();
    }

    int[][] dp = new int[origin.length() + 1][target.length() + 1];

    for (int i = 0; i <= origin.length(); i++) {
        dp[i][0] = i;
    }

    for (int j = 0; j <= target.length(); j++) {
        dp[0][j] = j;
    }

    for (int i = 1; i <= origin.length(); i++) {
        for (int j = 1; j <= target.length(); j++) {
            if (origin.charAt(i - 1) == target.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            }

            dp[i][j] = Math.min(dp[i][j], Math.min(dp[i - 1][j] + 1, dp[i][j - 1] + 1));
        }
    }
    return dp[origin.length()][target.length()];
}
```

如果我们需要求两个字符串的相识度,则是:

``` java
public static float getSimilarity(String origin, String target) {

    if (TextUtils.isEmpty(origin) || TextUtils.isEmpty(target)) {
        return 0f;
    }

    return 1.0f - getEditDistance(origin, target) / (float) Math.max(origin.length(), target.length());
}
```
