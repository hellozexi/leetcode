# Description
Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

A substring is a contiguous sequence of characters within the string.

 

**Example 1:**
```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

**Example 2:**
```
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.
```
**Example 3:**
```
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.
```

Constraints:

* m == s.length
* n == t.length
* 1 <= m, n <= 105
* s and t consist of uppercase and lowercase English letters.
 

**Follow up:** Could you find an algorithm that runs in O(m + n) time?

# Solution
```java
public class Solution {
    public String minWindow(String s, String t) {
        /**
         *         "aaabbbnnneaccab";
         *         "abcce"
         */
        int minLen = Integer.MAX_VALUE;
        int count = 0;
        int[] idx = new int[]{-1, -1};
        HashMap<Character, Integer> map = new HashMap<>();
        for(int i = 0; i < t.length(); i++) {
            map.put(t.charAt(i), map.getOrDefault(t.charAt(i), 0) + 1);
            count++;
        }
        int l = 0;
        int r = 0;
        while (r < s.length()) {
            if (map.get(s.charAt(r)) != null) {
                map.put(s.charAt(r), map.getOrDefault(s.charAt(r), 0) - 1);
                if (map.get(s.charAt(r)) >= 0) {
                    count--;
                }

            }
            while (count == 0) {
                if (minLen > r - l + 1) {
                    minLen = r - l + 1;
                    idx[0] = l;
                    idx[1] = r;
                }
                if (map.containsKey(s.charAt(l))) {
                    map.put(s.charAt(l), map.get(s.charAt(l)) + 1);
                    if (map.get(s.charAt(l)) > 0) {
                        count++;
                    }
                }
                l++;
            }
            r++;
        }
        if (idx[0] == -1 || idx[1] == -1) {
            return "";
        }
        return s.substring(idx[0], idx[1] + 1);
    }

    public static void main(String... args) {
        Solution s = new Solution();
        String res = s.minWindow("aaabbbnnneaccab", "abcce");
        System.out.println(res);
    }
}
```
# Complexity
```
time complexity = O(s + t)
space complexity = O(t)
```


# Solution2
```java
class Solution {
    public Map<Character, Integer> targetCounter = new HashMap<>();
    public String rst = "";
    public String minWindow(String s, String t) {
        if (t.length() > s.length()) return "";
        Map<Character, Integer> counte = new HashMap<>();
        for(char c: t.toCharArray()) {
            targetCounter.computeIfPresent(c, (k,v) -> v = v+1);
            targetCounter.putIfAbsent(c, 1);
            counte.put(c, 0);
        }
        Map<Character, Integer> counter = new HashMap<>(counte);
        for (int i = 0; i<t.length(); i++) {
            counter.computeIfPresent(s.charAt(i), (k,v) -> v = v+1);
        }
        int l=0,r=t.length()-1;
        while(r-l+1 >= t.length() && r < s.length()) {
            boolean flag = true;
            for (Map.Entry<Character, Integer> en : targetCounter.entrySet()) {
                if (counter.getOrDefault(en.getKey(), 0) < en.getValue()) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                if (rst.length() == 0 || r-l+1 < rst.length()) {
                    rst = s.substring(l, r+1);
                }
                counter.computeIfPresent(s.charAt(l), (k,v) -> v = v-1);
                l++;
            } else {
                r++;
                if (!(r-l+1 >= t.length() && r < s.length())) break;
                counter.computeIfPresent(s.charAt(r), (k,v) -> v = v+1);
            }
        }
        return rst;
    }
}
```
