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
state: f[i][j]从起点到i,j的最小路径和  
function: f[i][j] = min(f[i-1][j], f[i][j-1]) + grid[i][j]  
initialize: f[0][0] = grid[0][0], f[i][0] = f[i-1][0] + grid[i][0], f[0][j] = f[0][j-1] + grid[0][j]  
answer: f[m-1][n-1]

``` java
public class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int[][] answer = new int[m][n];

        answer[0][0] = grid[0][0];
        for (int i = 1; i < m; i++) {
            answer[i][0] = answer[i - 1][0] + grid[i][0];
        }
        for (int j = 1; j < n; j++) {
            answer[0][j] = answer[0][j - 1] + grid[0][j];
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                answer[i][j] = Math.min(answer[i - 1][j], answer[i][j - 1]) + grid[i][j];
            }
        }

        return answer[m - 1][n - 1];
    }
}
```

<hr />
## Triangle
https://leetcode.com/problems/triangle/

思路：  
state: 自底向上，f[i][j]表示从i,j到底层的最短路径和  
function: f[i][j] = min(f[i+1][j], f[i+1][j+1]) + triangle[i][j]  
initialize: f[n-1][j] = triangle[i-1][j]  
answer: f[0][0]

``` java
public class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[] answer = new int[n];

        for (int j = 0; j < n; j++) {
            answer[j] = triangle.get(n - 1).get(j);
        }

        for (int i = n - 2; i >= 0; i--) {
            for (int j = 0; j <= i; j++) {  // 第i层有i+1个结点（0层-1结点、1层-2结点...）
                answer[j] = Math.min(answer[j], answer[j + 1]) + triangle.get(i).get(j);
            }
        }

        return answer[0];
    }
}
```

<hr />
## Jump Game
https://leetcode.com/problems/jump-game/

思路：  
1. DP
2. Greedy  
维护right，表示从起始点能跳到的右边最远的点  
从左向右扫描，当前点i能跳到最远的点curRight = nums[i] + i  
right = max(right, curRight);  
right >= nums.length - 1表示right可跳到终点，返回true  
若i > right，表示从起始点跳不到i，则也跳不到终点

``` java
public class Solution {
    public boolean canJump(int[] nums) {
        int right = 0;
        for (int i = 0; i <= right && i < nums.length; i++) {
            right = Math.max(right, nums[i] + i);
            if (right >= nums.length - 1) {
                return true;
            }
        }
        return false;
    }
}
```

<hr />
## Jump Game II
https://leetcode.com/problems/jump-game-ii/

``` java
public class Solution {
    public int jump(int[] nums) {
        if (nums == null || nums.length == 0)
            return 0;

        int lastReach = 0;
        int reach = 0;
        int step = 0;

        for (int i = 0; i <= reach && i < nums.length; i++) {
            //when last jump can not read current i, increase the step by 1
            if (i > lastReach) {
                step++;
                lastReach = reach;
            }
            //update the maximal jump 
            reach = Math.max(reach, nums[i] + i);
        }

        if (reach < nums.length - 1)
            return 0;

        return step;
    }
}
```

<hr />
## Word Break
https://leetcode.com/problems/word-break/

思路：  
state: possible[i]表示字符串S的[0,i]子串可以被segmented by dict, 0 < i < S.length  
function: possible[i] = possible[k] && dict.contains(s.substr(k+1, i-k)), 0 < k < i  
initialize: possible[0] = true  
answer: possible[S.length]

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
