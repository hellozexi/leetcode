# Description

There are several stones arranged in a row, and each stone has an associated value which is an integer given in the array stoneValue.

In each round of the game, Alice divides the row into two non-empty rows (i.e. left row and right row), then Bob calculates the value of each row which is the sum of the values of all the stones in this row. Bob throws away the row which has the maximum value, and Alice's score increases by the value of the remaining row. If the value of the two rows are equal, Bob lets Alice decide which row will be thrown away. The next round starts with the remaining row.

The game ends when there is only one stone remaining. Alice's is initially zero.

Return the maximum score that Alice can obtain.

 

**Example 1:**
```
Input: stoneValue = [6,2,3,4,5,5]
Output: 18
```
Explanation: In the first round, Alice divides the row to [6,2,3], [4,5,5]. The left row has the value 11 and the right row has value 14. Bob throws away the right row and Alice's score is now 11.
In the second round Alice divides the row to [6], [2,3]. This time Bob throws away the left row and Alice's score becomes 16 (11 + 5).
The last round Alice has only one choice to divide the row which is [2], [3]. Bob throws away the right row and Alice's score is now 18 (16 + 2). The game ends because only one stone is remaining in the row.
**Example 2:**
```
Input: stoneValue = [7,7,7,7,7,7,7]
Output: 28
```
**Example 3:**
```
Input: stoneValue = [4]
Output: 0
```

**Constraints:**

* 1 <= stoneValue.length <= 500
* 1 <= stoneValue[i] <= 10^6

# Solution 1
```java
class Solution {
    public int stoneGameV(int[] stoneValue) {
        int n = stoneValue.length;
        int[] prefix = Arrays.copyOf(stoneValue, n);
        for (int i = 1; i < n; i++) {
            prefix[i] += prefix[i - 1]; 
        }
        int[][] memo = new int[n][n];
        return dfs(prefix, 0, n - 1, memo);
    }
    
    public int dfs(int[] prefix, int l, int r, int[][] memo) {
        if (l == r) {
            return 0;
        }
        if (memo[l][r] > 0) {
            return memo[l][r];
        }
        int res = 0;
        for (int i = l; i < r; i++) {
            int leftSum = prefix[i] - (l > 0 ? prefix[l - 1] : 0);
            int rightSum = prefix[r] - prefix[i];
            if (leftSum < rightSum) {
                res = Math.max(res, leftSum + dfs(prefix, l, i, memo));
            } else if (leftSum > rightSum) {
                res = Math.max(res, rightSum + dfs(prefix, i + 1, r, memo));
            } else {
                res = Math.max(res, Math.max(leftSum + dfs(prefix, l, i, memo), rightSum + dfs(prefix, i + 1, r, memo)));
            }
        }
        memo[l][r] = res;
        return res;
    
    }
}

```

# Solution2
```java
class Solution {
    public int[] sum;

    public int dp(int[] stoneValue, int l, int r, Integer[][] cache) {
        if (l == r) return 0;
        if (cache[l][r] != null) return cache[l][r];
        int max = 0;
        for (int i = l; i < r; i++) {
            int leftSum =  l < 1 ? sum[i] : sum[i]-sum[l-1];
            int rightSum = sum[r] - sum[i];
            int tmpMax = 0;
            if (2*Math.min(leftSum, rightSum) < max) continue;
            if (leftSum > rightSum) {
                tmpMax = rightSum + dp(stoneValue, i + 1, r, cache);
            } else if (leftSum < rightSum) {
                tmpMax = leftSum + dp(stoneValue, l, i, cache);
            } else {
                tmpMax = Math.max(leftSum + dp(stoneValue, l, i, cache), rightSum + dp(stoneValue, i + 1, r, cache));
            }
            max = Math.max(max, tmpMax);
        }
        cache[l][r] = max;
        return max;
    }

    public int stoneGameV(int[] stoneValue) {
        if (stoneValue.length <= 1) {
            return 0;
        }
        sum = new int[stoneValue.length];
        sum[0] = stoneValue[0];
        for(int i=1;i<stoneValue.length;i++) {
            sum[i] += sum[i-1] + stoneValue[i];
        }
        return dp(stoneValue, 0, stoneValue.length-1,  new Integer[stoneValue.length][stoneValue.length]);
    }
}
```