# Description
In a string s of lowercase letters, these letters form consecutive groups of the same character.

For example, a string like s = "abbxxxxzyy" has the groups "a", "bb", "xxxx", "z", and "yy".

A group is identified by an interval [start, end], where start and end denote the start and end indices (inclusive) of the group. In the above example, "xxxx" has the interval [3,6].

A group is considered large if it has 3 or more characters.

Return the intervals of every large group sorted in increasing order by start index.

 

**Example 1:**
```
Input: s = "abbxxxxzzy"
Output: [[3,6]]
Explanation: "xxxx" is the only large group with start index 3 and end index 6.
```
**Example 2:**
```
Input: s = "abc"
Output: []
Explanation: We have groups "a", "b", and "c", none of which are large groups.
```
**Example 3:**
```
Input: s = "abcdddeeeeaabbbcd"
Output: [[3,5],[6,9],[12,14]]
Explanation: The large groups are "ddd", "eeee", and "bbb".
```
**Example 4:**
```
Input: s = "aba"
Output: []
 ```

Constraints:

1 <= s.length <= 1000
s contains lower-case English letters only.

# Solution
```java
class Solution {
    public List<List<Integer>> largeGroupPositions(String s) {
        List<List<Integer>> res = new ArrayList<>();
        int i = 0;
        int j = 0;
        int len = 0;
        while (i < s.length()) {
            if (s.charAt(i) != s.charAt(j)) {
                len = i - j;
                if (len >= 3) {
                    res.add(Arrays.asList(j, i - 1));
                }
                j = i;
                len = 0;
            }
            i++;
        }
        if (i - j >= 3) {
            res.add(Arrays.asList(j, i - 1));
        }
        return res;
    }
}

```

# Solution2
```java
class Solution {
    public List<List<Integer>> largeGroupPositions(String s) {
        List<List<Integer>> rst = new ArrayList<>();
        if (s.length() < 3) return rst;
        int last = 0;
        int counter = 1;
        for(int i = 1; i<s.length();i++) {
            if (s.charAt(i) == s.charAt(last)) {
                counter++;
            } else {
                if (counter >= 3) {
                    rst.add(Arrays.asList(last, i-1));
                }
                counter = 1;
                last = i;
            }
        }
        if (counter >= 3) {
            rst.add(Arrays.asList(last, s.length()-1));
        }
        return rst;
    }
}
```