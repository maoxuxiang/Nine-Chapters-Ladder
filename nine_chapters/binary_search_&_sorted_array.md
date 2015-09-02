# Binary Search & Sorted Array

- [Merge Sorted Array](#merge-sorted-array)
- [Search Insert Position](#search-insert-position)
- [Recover Rotated Sorted Array](#recover-rotated-sorted-array)
- [Binary Search](#binary-search)
- [Find Peak Element](#find-peak-element)
- [First Bad Version](#first-bad-version)
- [Search in Rotated Sorted Array](#search-in-rotated-sorted-array)
- [Search for a Range](#search-for-a-range)
- [Median of two Sorted Arrays](#median-of-two-sorted-arrays)

<hr />
## Merge Sorted Array
https://leetcode.com/problems/merge-sorted-array/

``` java
public class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;
        while (k >= 0) {
            if (j < 0 || (i >= 0 && nums1[i] > nums2[j])) {
                nums1[k--] = nums1[i--];
            } else {
                nums1[k--] = nums2[j--];
            }
        }
    }
}
```

<hr />
## Search Insert Position
https://leetcode.com/problems/search-insert-position/

思路：  
二分查找，注意边界条件  
1. 循环跳出条件：`low <= high`
2. 退出条件：`mid / mid + 1`

```java
public class Solution {
    public int searchInsert(int[] nums, int target) {
        if (nums.length == 0) {
            return 0;
        }
        int low = 0, high = nums.length - 1;
        int mid = low + (high - low) / 2;
        while (low <= high) {
            mid = low + (high - low) / 2;
            if (nums[mid] < target) {
                low = mid + 1;
            } else if (nums[mid] > target) {
                high = mid - 1;
            } else {
                return mid;
            }
        }
        if (nums[mid] < target) {
            return mid + 1;
        } else {
            return mid;
        }
    }
}
```

<hr />
## Recover Rotated Sorted Array
http://www.lintcode.com/en/problem/recover-rotated-sorted-array/

```java
public class Solution {
    /**
     * @param nums: The rotated sorted array
     * @return: void
     */
    public void recoverRotatedSortedArray(ArrayList<Integer> nums) {
        for (int i = 1; i < nums.size(); i++) {
            if (nums.get(i) < nums.get(i - 1)) {
                reverse(nums, 0, i - 1);
                reverse(nums, i, nums.size() - 1);
                reverse(nums, 0, nums.size() - 1);
            }
        }
    }

    private void reverse(List<Integer> nums, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            int temp = nums.get(i);
            nums.set(i, nums.get(j));
            nums.set(j, temp);
        }
    }
}
```

<hr />
## Binary Search
http://www.lintcode.com/en/problem/binary-search/

``` java
class Solution {
    /**
     * @param nums: The integer array.
     * @param target: Target to find.
     * @return: The first position of target. Position starts from 0.
     */
    public int binarySearch(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        int low = 0, high = nums.length - 1, mid;
        while (low <= high) {
            mid = low + (high - low) / 2;
            if (nums[mid] < target) {
                low = mid + 1;
            } else if (nums[mid] > target) {
                high = mid - 1;
            } else {
                while (mid > 0) {
                    if (nums[mid] == nums[mid - 1]) {
                        mid--;
                    } else {
                        break;
                    }
                }
                return mid;
            }
        }
        return -1;
    }
}
```

<hr />
## Find Peak Element
https://leetcode.com/problems/find-peak-element/

思路：  
二分查找  
循环结束条件：`low == high`  
始终拿`mid`和`mid+1`比较：  
若升序(`nums[mid] < nums[mid + 1]`)，则峰值在`mid+1`右侧，`low = mid + 1`  
若降序(`nums[mid] >= nums[mid + 1]`), 则峰值在mid左侧，`high = mid`

``` java
public class Solution {
    public int findPeakElement(int[] nums) {
        int low = 0, high = nums.length - 1, mid;
        while (low <= high) {
            if (low == high) {
                return low;
            }
            mid = low + (high - low) / 2;
            if (nums[mid] < nums[mid + 1]) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return -1;
    }
}
```

<hr />
## First Bad Version
http://www.lintcode.com/en/problem/first-bad-version/

``` java
/**
 * public class VersionControl {
 *     public static boolean isBadVersion(int k);
 * }
 * you can use VersionControl.isBadVersion(k) to judge whether
 * the kth code version is bad or not.
*/
class Solution {
    /**
     * @param n: An integers.
     * @return: An integer which is the first bad version.
     */
    public int findFirstBadVersion(int n) {
        int low = 1, high = n, mid;
        while (low <= high) {
            if (low == high) {
                return high;
            }
            mid = low + (high - low) / 2;
            if (VersionControl.isBadVersion(mid)) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return -1;
    }
}
```

<hr />
## Search in Rotated Sorted Array
https://leetcode.com/problems/search-in-rotated-sorted-array/

思路：  
参考[水中的鱼](http://fisherlei.blogspot.com/2013/01/leetcode-search-in-rotated-sorted-array.html)
![](http://3.bp.blogspot.com/-ovV6zYeEdZg/U_ke6coEoAI/AAAAAAAAIc4/lmb1A9FsjgQ/s1600/Picture123.png)

``` java
public class Solution {
    public int search(int[] nums, int target) {
        int low = 0, high = nums.length - 1, mid;
        while (low <= high) {
            mid = low + ((high - low) >> 1);
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[low] <= nums[mid]) {
                if (nums[low] <= target && target < nums[mid]) {
                    high = mid - 1;
                } else {
                    low = mid + 1;
                }
            } else {
                if (nums[mid] < target && target <= nums[high]) {
                    low = mid + 1;
                } else {
                    high = mid - 1;
                }
            }
        }
        return -1;
    }
}
```

<hr />
## Search for a Range
https://leetcode.com/problems/search-for-a-range/

思路：  
二分查找位置  
向左走找到最左  
向右走找到最右

``` java
public class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] result = new int[2];
        int index = binarySearch(nums, target);
        if (index == -1) {
            result[0] = -1;
            result[1] = -1;
            return result;
        }
        int left = index;
        while (left > 0 && nums[left] == nums[left - 1]) {
            left--;
        }
        int right = index;
        while (right < nums.length - 1 && nums[right] == nums[right + 1]) {
            right++;
        }
        result[0] = left;
        result[1] = right;
        return result;
    }

    private int binarySearch(int[] nums, int target) {
        int low = 0, high = nums.length - 1, mid;
        while (low <= high) {
            mid = low + ((high - low) >> 1);
            if (target > nums[mid]) {
                low = mid + 1;
            } else if (target < nums[mid]) {
                high = mid - 1;
            } else {
                return mid;
            }
        }
        return -1;
    }
}
```

<hr />
## Median of Two Sorted Arrays
https://leetcode.com/problems/median-of-two-sorted-arrays/

思路：  
两个有序数组的中位数$$k = (m + n) / 2$$  
则可将题目转化为**求两个有序数组中第$$k$$大的数（Kth element in 2 sorted array）**  
比较$$A_{k/2}$$与$$B_{k/2}$$（若数组长度小于$$k/2$$，则记为无穷大）：  
若$$A_{k/2} < B_{k/2}$$，表示$$A_0,\ldots,A_{k/2}$$中的元素都在$$A$$和$$B$$合并之后的前$$k$$小的元素中，说明第$$k$$个数不在$$A_0,\ldots,A_{k/2}$$中，移动指针指向$$A_{k+1}$$，在$$A_{k+1},\ldots,A_m$$和$$B$$数组中**找第$$k-k/2$$大的数**（相当于删除了$$k/2$$个元素）  
否则，说明第$$k$$个数不在$$B_0,\ldots,B_{k/2}$$中，移动指针指向$$B_{k+1}$$，在$$B_{k+1},\ldots,B_n$$和$$A$$数组中**找第$$k-k/2$$大的数**（相当于删除了$$k/2$$个元素）

递归退出条件：

递归有三个参数，在A中查找的起始位置`start1`、在B中查找的起始位置`start2`、`k`

则两个有序数组中找第`k`大的数的退出条件有三个：

1. `start1 == m`
返回`B`中第`k-1`大的数
2. `start2 == n`
返回`A`中第`k-1`大的数
3. `k == 1`
返回`A`、`B`相应位置较小的那个数

``` java
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int length = nums1.length + nums2.length;
        if (length % 2 == 0) {
            return (findKth(nums1, 0, nums2, 0, length / 2) + findKth(nums1, 0, nums2, 0, length / 2 + 1)) / 2;
        } else {
            return findKth(nums1, 0, nums2, 0, length / 2 + 1);
        }
    }

    private double findKth(int[] nums1, int start1, int[] nums2, int start2, int k) {
        if (start1 >= nums1.length) {
            return nums2[start2 + k - 1];
        }
        if (start2 >= nums2.length) {
            return nums1[start1 + k - 1];
        }
        if (k == 1) {
            return Math.min(nums1[start1], nums2[start2]);
        }

        int key1 = start1 + k / 2 - 1 < nums1.length ? nums1[start1 + k / 2 - 1] : Integer.MAX_VALUE;
        int key2 = start2 + k / 2 - 1 < nums2.length ? nums2[start2 + k / 2 - 1] : Integer.MAX_VALUE;

        if (key1 < key2) {
            return findKth(nums1, start1 + k / 2, nums2, start2, k - k / 2);
        } else {
            return findKth(nums1, start1, nums2, start2 + k / 2, k - k / 2);
        }
    }
}
```
