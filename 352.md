
# Description
Given a data stream input of non-negative integers a1, a2, ..., an, summarize the numbers seen so far as a list of disjoint intervals.

Implement the SummaryRanges class:

SummaryRanges() Initializes the object with an empty stream.
void addNum(int val) Adds the integer val to the stream.
int[][] getIntervals() Returns a summary of the integers in the stream currently as a list of disjoint intervals [starti, endi].
 

Example 1:

Input
```
["SummaryRanges", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals"]
[[], [1], [], [3], [], [7], [], [2], [], [6], []]
Output
[null, null, [[1, 1]], null, [[1, 1], [3, 3]], null, [[1, 1], [3, 3], [7, 7]], null, [[1, 3], [7, 7]], null, [[1, 3], [6, 7]]]
```
**Explanation**
```
SummaryRanges summaryRanges = new SummaryRanges();
summaryRanges.addNum(1);      // arr = [1]
summaryRanges.getIntervals(); // return [[1, 1]]
summaryRanges.addNum(3);      // arr = [1, 3]
summaryRanges.getIntervals(); // return [[1, 1], [3, 3]]
summaryRanges.addNum(7);      // arr = [1, 3, 7]
summaryRanges.getIntervals(); // return [[1, 1], [3, 3], [7, 7]]
summaryRanges.addNum(2);      // arr = [1, 2, 3, 7]
summaryRanges.getIntervals(); // return [[1, 3], [7, 7]]
summaryRanges.addNum(6);      // arr = [1, 2, 3, 6, 7]
summaryRanges.getIntervals(); // return [[1, 3], [6, 7]]
 ```

**Constraints:**

```0 <= val <= 104```
At most ```3 * 104``` calls will be made to addNum and getIntervals.
 

Follow up: What if there are lots of merges and the number of disjoint intervals is small compared to the size of the data stream?
## Solution1
``` java

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public static class SummaryRanges {

        public List<int[]> list;

        public SummaryRanges() {
            list = new ArrayList<>();
        }

        public void addNum(int val) {
            boolean linkPrev = false;
            if (list.size() == 0) {
                list.add(new int[]{val, val});
                return;
            }
            int size = list.size();
            for(int i = 0; i < size; i++) {
                int[] interval = list.get(i);
                int start = interval[0];
                int end = interval[1];
                if (val >= start && val <= end) {
                    return;
                } else if (val < start) {
                    if (val == start - 1) {
                        if (!linkPrev) {
                            list.remove(i);
                            list.add(i, new int[]{val, end});
                        } else {
                            int[] cur = list.remove(i);
                            int[] prev = list.remove(i - 1);

                            list.add(i - 1, new int[]{prev[0], cur[1]});
                        }
                        return;
                    } else {
                        if (linkPrev) {
                            return;
                        }
                        list.add(i, new int[]{val, val});
                        return;
                    }
                } else {
                    if (val == end + 1) {
                        list.remove(interval);
                        list.add(i, new int[]{start, val});
                        linkPrev = true;
                    } else {
                        if (i == size - 1) {
                            list.add(new int[]{val, val});
                            return;
                        }
                    }
                }
            }
        }

        public int[][] getIntervals() {
            int[][] res = new int[list.size()][2];
            list.toArray(res);
            return res;
        }

        public int[][] convert(List<List<Integer>> map) {
            int[][] res = new int[map.size()][2];
            for(int row = 0; row < res.length; row++) {
                res[row][0] = map.get(row).get(0);
                res[row][1] = map.get(row).get(1);
            }
            return res;
        }
    }

    public static void main(String[] args) {
        SummaryRanges summaryRanges = new SummaryRanges();
        summaryRanges.addNum(1);
        summaryRanges.addNum(9);
        summaryRanges.addNum(2);
        summaryRanges.addNum(5);
        summaryRanges.addNum(6);
        summaryRanges.addNum(7);
        summaryRanges.addNum(10);
        summaryRanges.addNum(10);
        summaryRanges.addNum(8);
        summaryRanges.addNum(0);
        int[][] res = summaryRanges.getIntervals();
        printHelper(res);
    }

    public static void printHelper(int[][] arr) {
        for(int i = 0; i < arr.length; i++) {
            System.out.println(arr[i][0] + "," + arr[i][1]);
            System.out.println("\n");
        }
    }
}
```
