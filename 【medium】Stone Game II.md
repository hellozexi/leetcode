# Description
Alice and Bob continue their games with piles of stones.  There are a number of piles arranged in a row, and each pile has a positive integer number of stones piles[i].  The objective of the game is to end with the most stones. 

Alice and Bob take turns, with Alice starting first.  Initially, M = 1.

On each player's turn, that player can take all the stones in the first X remaining piles, where 1 <= X <= 2M.  Then, we set M = max(M, X).

The game continues until all the stones have been taken.

Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

 

**Example 1:**
```
Input: piles = [2,7,9,4,4]
Output: 10
Explanation:  If Alice takes one pile at the beginning, Bob takes two piles, 
then Alice takes 2 piles again. Alice can get 2 + 4 + 4 = 10 piles in total. 
If Alice takes two piles at the beginning, then Bob can take all three piles left. 
In this case, Alice get 2 + 7 = 9 piles in total. So we return 10 since it's larger.
```
**Example 2:**
```
Input: piles = [1,2,3,4,5,100]
Output: 104
```

Constraints:
* 1 <= piles.length <= 100
* 1 <= piles[i] <= 104
# Solution
``` java
class Solution {
    public int stoneGameII(int[] piles) {
        int n = piles.length;
        int[] prefix = Arrays.copyOf(piles, n);
        for(int i = n -2; i >= 0; i--) {
            prefix[i] += prefix[i + 1];
        }
        int[][] memo = new int[n][n];
        return dfs(0, 1, prefix, memo);
    }
    
    /**
    start: 每次取石头最开始的index
    */
    public int dfs(int start, int M, int[] prefix, int[][] memo) {
        if (start >= prefix.length) {
            return 0;
        }
        if (start + 2 * M >= prefix.length) {
            return prefix[start];
        }
        if (memo[start][M] > 0) {
            return memo[start][M];
        }
        int res = 0;
        /**
          i是每次可以取到的石头数量，每次会判断下一个人取到的最大石头数量，下一个人取到的越小，那么最后的获取数越大
        */
        for (int i = 1; i <= 2 * M; i++) {
            res = Math.max(res, prefix[start] - dfs(start + i, Math.max(M, i), prefix, memo));
        }
        memo[start][M] = res;
        return res;
    }
}
```
