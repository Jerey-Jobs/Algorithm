### 最长公共子序列 (Longest Common Subsequence)
公共子序列与公共子串很像，只不过，子串代表的是连续的，而序列 代表的是一串不连续的字符串


### Code
```
import java.util.Scanner;

/**
 * 给定两个字符串，求解这两个字符串的最长公共子序列（Longest Common Sequence）。比如字符串1：BDCABA；字符串2：ABCBDAB
 * <p>
 * 则这两个字符串的最长公共子序列长度为4，最长公共子序列是：BCBA/BCAB/BDAB
 * <p>
 * 思路：动态规划
 */
public class LongestCommonSequence {

    public static void main(String[] args) {

        Scanner scanner = new Scanner(System.in);
        String str1 = scanner.nextLine().trim();
        String str2 = scanner.nextLine().trim();
        System.out.println(getLongestCommonSequence("BDCABA", "ABCBDAB"));
    }

    public static String getLongestCommonSequence(String str1, String str2) {
        if (str1 == null || str2 == null) {
            throw new IllegalArgumentException();
        }

        StringBuilder stringBuilder = new StringBuilder();
        int[][] table = new int[str1.length() + 1][str2.length() + 1];
        int i,j = 0;
        for (i = 1; i < str1.length() + 1; i++) {
            for (j = 1; j < str2.length() + 1; j++) {
                if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                    table[i][j] = table[i - 1][j - 1] + 1;
                } else {
                    table[i][j] = Math.max(table[i -1][j], table[i][j - 1]);
                }
            }
        }
        // 找到了最大的值了，现在要回溯路径
        i = str1.length();
        j = str2.length();
        while (table[i][j] != 0) {
            if (table[i][j] == table[i-1][j]) {
                i--;
            }
            else if (table[i][j] == table[i][j - 1]) {
                j--;
            } else if (table[i][j] >= table[i - 1][j - 1]) {
                i--;
                j--;

                stringBuilder.insert(0, str1.charAt(i));
                System.out.println(str1.charAt(i));
            }
        }
        return stringBuilder.toString();
    }
}

```
