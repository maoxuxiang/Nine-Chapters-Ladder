# Dynamic Programming I

- [Unique Paths](#unique-paths)
- [Unique Paths II](#unique-paths-ii)
- [Climbing Stairs](#climbing-stairs)
- [Minimum Path Sum](#minimum-path-sum)
- [Triangle](#triangle)
- [Jump Game](#jump-game)
- [Jump Game II](#jump-game-ii)
- [Word Break](#word-break)
- [Palindrome Partitioning II](#palindrome-partitioning-ii)
- [Longest Increasing Subsequence](#longest-increasing-subsequence)

<hr />
## Unique Paths
https://leetcode.com/problems/unique-paths/

思路：

state: f[i][j]从起点到i,j的路径数

function: f[i][j] = f[i-1][j] + f[i][j-1]

initialize: f[i][0] = 1, f[0][j] = 1

answer: f[m-1][n-1]

``` java
public class Solution {
    public int uniquePaths(int m, int n) {
        int[][] array = new int[m][n];
        for (int i = 0; i < m; i++) {
            array[i][0] = 1;
        }
        for (int j = 0; j < n; j++) {
            array[0][j] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                array[i][j] = array[i - 1][j] + array[i][j - 1];
            }
        }
        return array[m - 1][n - 1];
    }
}
```

<hr />
## Unique Paths II
https://leetcode.com/problems/unique-paths-ii/

思路：

state: f[i][j]从起点到i,j的路径数

function: f[i][j] = f[i-1][j] + f[i][j-1]

initialize: f[i][0] = 1, f[0][j] = 1

answer: f[m-1][n-1]

obstacleGrid[i][j] = 1表示路径不通，则grid[i][j] = 0

``` java
public class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] array = new int[m][n];
        
        for (int i = 0; i < m; i++) {
            if (obstacleGrid[i][0] == 1) {
                break;
            }
            array[i][0] = 1;
        }
        for (int j = 0; j < n; j++) {
            if (obstacleGrid[0][j] == 1) {
                break;
            }
            array[0][j] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 1) {
                    array[i][j] = 0;
                } else {
                    array[i][j] = array[i - 1][j] + array[i][j - 1];
                }
            }
        }
        
        return array[m - 1][n - 1];
    }
}
```

<hr />
## Climbing Stairs
https://leetcode.com/problems/climbing-stairs/

思路：

state: f[n]从起点到n的方式数量

function: f[n] = f[n-1] + f[n-2]

initialize: f[0] = 1, f[1] = 1, f[2] = f[0] + f[1] = 2

answer: f[n]

``` java
public class Solution {
    public int climbStairs(int n) {
        int first = 1;
        int second = 1;
        if (n <= 1) {
            return second;
        }
        
        int result = 0;
        for (int i = 1; i < n; i++) {
            result = first + second;
            first = second;
            second = result;
        }
        return result;
    }
}
```

<hr />
## Minimum Path Sum
https://leetcode.com/problems/minimum-path-sum/

思路：

``` java

```

<hr />
## Triangle
https://leetcode.com/problems/triangle/

思路：

``` java

```

<hr />
## Jump Game
https://leetcode.com/problems/jump-game/

思路：

``` java

```

<hr />
## Jump Game II
https://leetcode.com/problems/jump-game-ii/

思路：

``` java

```

<hr />
## Word Break
https://leetcode.com/problems/word-break/

思路：

``` java

```

<hr />
## Palindrome Partitioning II
https://leetcode.com/problems/palindrome-partitioning-ii/

思路：

``` java

```

<hr />
## Longest Increasing Subsequence
http://www.lintcode.com/en/problem/longest-increasing-subsequence/

思路：

``` java

```
