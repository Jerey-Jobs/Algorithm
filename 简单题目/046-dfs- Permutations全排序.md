### 问题

* 求全排序
* 如:
*[1,2,3] have the following permutations:
*
* [
*   [1,2,3],
*   [1,3,2],
*   [2,1,3],
*   [2,3,1],
*   [3,1,2],
*   [3,2,1]
* ]

### 问题分析
全排序问题,使用深度搜索来解决是最简单的.
但是需要注意一个点: 回溯

### code
``` java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * 求全排序
 * 如:
 *[1,2,3] have the following permutations:
 *
 * [
 *   [1,2,3],
 *   [1,3,2],
 *   [2,1,3],
 *   [2,3,1],
 *   [3,1,2],
 *   [3,2,1]
 * ]
 */

class Permute {
    public static void main(String[] args) {
        System.out.println(permute(new int[]{1,2,3}));
    }

    public static List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> listList = new ArrayList<>();
        permuteAction(nums, new ArrayList<>(), listList);

        boolean[] tb = new boolean[nums.length];
        permuteAction2(nums, tb,new ArrayList<>(), listList);
        return listList;
    }


    public static void permuteAction(int[] numbs, ArrayList<Integer> list ,List<List<Integer>> lists) {
        if (list.size() >= numbs.length) {
            lists.add((List<Integer>) list.clone());
            return;
        }

        for (int i = 0; i < numbs.length; i++) {
            if (list.contains(numbs[i]) ) {
                continue;
            }

            list.add(numbs[i]);
            // System.out.println(list);
            permuteAction(numbs, list, lists);
            list.remove(list.size() - 1);
        }
    }

    /**
     * 利用表tb, 来记录在每次的递归中,谁已经用过了. 避免重复
     */
    public static void permuteAction2(int[] numbs, boolean[] tb, ArrayList<Integer> list ,List<List<Integer>> lists) {

        if (list.size() >= numbs.length) {
            lists.add((List<Integer>) list.clone());
            return;
        }

        for (int i = 0; i < numbs.length; i++) {
            if (!tb[i]) {
                list.add(numbs[i]);
                tb[i] = true;
                System.out.println(Arrays.toString(tb) + " " + list);
                permuteAction2(numbs, tb, list, lists);
                tb[i] = false;
                list.remove(list.size() - 1);
            }
        }
    }
}


```
