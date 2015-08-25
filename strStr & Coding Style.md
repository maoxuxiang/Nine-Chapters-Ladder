<!-- toc -->

- [Implement strStr()](https://leetcode.com/problems/implement-strstr/)
- [Subsets](https://leetcode.com/problems/subsets/)
- [Subsets II](https://leetcode.com/problems/subsets-ii/)
- [Permutations](https://leetcode.com/problems/permutations/)
- [Permutations II](https://leetcode.com/problems/permutations-ii/)
- [Permutation Sequence](https://leetcode.com/problems/permutation-sequence/)

<hr />
## [Implement strStr()](https://leetcode.com/problems/implement-strstr/)

``` java
public class Solution {
    public int strStr(String haystack, String needle) {
        if (haystack == null || needle == null) {
            return -1;
        }

        int i, j;
        for (i = 0; i < haystack.length() - needle.length() + 1; i++) {
            for (j = 0; j < needle.length(); j++) {
                if (haystack.charAt(i + j) != needle.charAt(j))
                    break;
            }
            if (j == needle.length()) {
                return i;
            }
        }
        return -1;
    }
}
```

<hr />
## [Subsets](https://leetcode.com/problems/subsets/)

思路：
递归构造

``` java
public class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> res = new ArrayList<>();
        Arrays.sort(nums);
        subsetsHelper(result, res, nums, 0);
        return result;
    }

    private void subsetsHelper(List<List<Integer>> result, List<Integer> res, int[] nums, int pos) {
        result.add(new ArrayList<>(res));
        for (int i = pos; i < nums.length; i++) {
            res.add(nums[i]);
            subsetsHelper(result, res, nums, i + 1);
            res.remove(res.size() - 1);
        }
    }
}
```

<hr />
## [Subsets II](https://leetcode.com/problems/subsets-ii/)

``` java
public class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> res = new ArrayList<>();
        Arrays.sort(nums);
        subsetsHelper(result, res, nums, 0);
        return result;
    }

    private void subsetsHelper(List<List<Integer>> result, List<Integer> res, int[] nums, int pos) {
        result.add(new ArrayList<>(res));
        for (int i = pos; i < nums.length; i++) {
            if (i != pos && nums[i] == nums[i - 1]) {
                continue;
            }
            res.add(nums[i]);
            subsetsHelper(result, res, nums, i + 1);
            res.remove(res.size() - 1);
        }
    }
}
```

<hr />
## [Permutations](https://leetcode.com/problems/permutations/)

思路：
visited[]记录该位置是否访问过

``` java
public class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null) {
            return result;
        }

        boolean[] visited = new boolean[nums.length];
        for (int i = 0; i < visited.length; i++) {
            visited[i] = false;
        }

        List<Integer> res = new ArrayList<>();
        permuteHelper(result, res, nums, visited);
        return result;
    }

    private void permuteHelper(List<List<Integer>> result, List<Integer> res, int[] nums, boolean[] visited) {
        if (res.size() == nums.length) {
            result.add(new ArrayList<>(res));
        }
        for (int i = 0; i < nums.length; i++) {
            if (visited[i]) {
                continue;
            }
            res.add(nums[i]);
            visited[i] = true;
            permuteHelper(result, res, nums, visited);
            res.remove(res.size() - 1);
            visited[i] = false;
        }
    }
}
```

<hr />
## [Permutations II](https://leetcode.com/problems/permutations-ii/)

思路：
注意循环跳过条件

``` java
public class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null) {
            return result;
        }

        Arrays.sort(nums);
        List<Integer> res = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];
        permuteUniqueHelper(result, res, nums, visited);
        return result;
    }

    private void permuteUniqueHelper(List<List<Integer>> result, List<Integer> res, int[] nums, boolean[] visited) {
        if (res.size() == nums.length) {
            result.add(new ArrayList<>(res));
        }
        for (int i = 0; i < nums.length; i++) {
            if (visited[i] || (i > 0 && nums[i] == nums[i - 1] && !visited[i - 1])) {
                continue;
            }
            res.add(nums[i]);
            visited[i] = true;
            permuteUniqueHelper(result, res, nums, visited);
            res.remove(res.size() - 1);
            visited[i] = false;
        }
    }
}
```

<hr />
## [Permutation Sequence](https://leetcode.com/problems/permutation-sequence/)

思路一：
递归构造所有排列，返回第k个

思路二：
找数学规律
假设有$n$个元素，第$k$个permutation是$a_1a_2\ldots a_n$
对于$a_1$开头的排列，$a_2...a_n$共$n-1$个元素，有$(n-1)!$种排列
令$i = k / (n-1)!$，表示第$k$个permutation在第$i$组，$a_1 = k / (n-1)!$
令$k' = k \% (n-1)!$，表示第$k$个permutation在第$i$组的$k'$位置
则有：
> $k_1 = k$
$a_1 = k_1 / (n-1)!$
$k_2 = k_1 \% (n-1)!$
$a_2 = k_2 / (n-2)!$
$\ldots$

``` java
public class Solution {
    public String getPermutation(int n, int k) {
        StringBuilder result = new StringBuilder();
        List<Integer> nums = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            nums.add(i);
        }

        k--;
        int factorial = 1;
        for (int i = 2; i <= n; i++) {
            factorial *= i;
        }

        for (int i = 0; i < n; i++) {
            factorial /= n - i; // factorial = (n-i-1)!
            int index = k / factorial;
            result.append(nums.get(index));
            nums.remove(index);
            k %= factorial;
        }

        return result.toString();
    }
}
```
