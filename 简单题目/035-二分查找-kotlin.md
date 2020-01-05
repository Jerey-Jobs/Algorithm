### Search Insert Position
一个已排序的数组,二分查找 找到对应插入点


###
```
fun main(args: Array<String>) {
    print(searchInsert(arrayOf(1, 3, 5, 6, 12, 54), 7))
}

fun searchInsert(nums: Array<Int>, target: Int): Int {
    var left = 0
    var right = nums.size
    var mid = (left + right).shr(1)
    while (left <= right) {
        if (target <= nums[mid]) {
            right = mid - 1
        } else {
            left = mid + 1
        }
        mid = (left + right).shr(1)
    }
    return left
}
```
