### 问题
给出一个区间的集合，请合并所有重叠的区间。

示例 1:

输入: [[1,3],[2,6],[8,10],[15,18]] <br>
输出: [[1,6],[8,10],[15,18]] <br>
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].<br>

示例 2:

输入: [[1,4],[4,5]]<br>
输出: [[1,5]]<br>
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。

### 分析
1.将所有区间排序
2.合并相邻区间


### Code
``` kotlin
import java.lang.RuntimeException
import java.util.*
import kotlin.collections.ArrayList
import kotlin.math.max
import kotlin.reflect.jvm.internal.impl.utils.CollectionsKt

fun main(args: Array<String>) {
    var mList = ArrayList<MergeInterval.Interval>()
    mList.add(MergeInterval.Interval(2,5))
    mList.add(MergeInterval.Interval(8,9))
    mList.add(MergeInterval.Interval(1,3))
    mList.add(MergeInterval.Interval(1,6))
    mList.add(MergeInterval.Interval(12,15))
    MergeInterval().mergeInterVal(mList)
}


final class MergeInterval {
    class Interval(start: Int) {
        constructor(start: Int, end: Int) : this(start) {
            if (start > end) {
                throw RuntimeException("error start and end")
            }
            this.mStart = start
            this.mEnd = end
        }

        var mStart: Int = 0
        var mEnd: Int = 0

        override fun toString(): String {
            return "[$mStart, $mEnd]"
        }
    }


    fun mergeInterVal(intervals: List<Interval>) : List<Interval>{
        var list = intervals.sortedBy { it.mStart }
        list.forEach(::println)

        var retList = ArrayList<Interval>()

        var start = list[0].mStart;
        var end = list[0].mEnd;
        for ((index, value) in list.withIndex()) {
            if (end > value.mStart) {
                end = max(end, value.mEnd)
            } else {
                retList.add(Interval(start, end))
                start = value.mStart;
                end = value.mEnd
            }
        }
        retList.add(Interval(start, end))
        retList.forEach(::println)
        return retList
    }
}
```
