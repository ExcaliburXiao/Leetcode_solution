# Leetcode 个人题解 

该文件用于笔者刷题期间记录思路

刷题顺序，参考：https://noworneverev.github.io/leetcode_101/

## 1 贪心



## 2 双指针



## 3 二分查找

### [154. Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)

思路：

代码：

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums)-1
        while left < right:
            mid = (left+right)//2
            if nums[left] < nums[right]:
                return nums[left]
            if nums[mid] > nums[left] >= nums[right]:
                left = mid + 1
            elif nums[mid] < nums[right] <= nums[left]:
                right = mid
            elif nums[mid] == nums[left]:
                left += 1
            elif nums[mid] == nums[right]:
                right -= 1
        return nums[left]
```



### [540. Single Element in a Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array/)

思路：

代码：

```python
class Solution:
    def singleNonDuplicate(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]
        left, right = 0, len(nums)-1
        while left <= right:
            mid = (left + right)//2
            if mid == 0 and nums[mid] != nums[mid+1]:
                return nums[mid]
            if mid == len(nums) - 1 and nums[mid] != nums[mid-1]:
                return nums[mid]
            if nums[mid]!=nums[mid-1] and nums[mid] != nums[mid+1]:
                return nums[mid]
            if mid % 2 == 1:
                if nums[mid] == nums[mid+1]:
                    right = mid - 1
                else:
                    left = mid + 1
            else:
                if nums[mid] == nums[mid-1]:
                    right = mid - 1
                else:
                    left = mid + 1
```



### [4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

思路：对于总长度为奇数的情况，只需要求第 `total_len//2`小的元素；对于总长度为偶数的情况，需要求第 `total_len//2`和 `total_len//2-1`小的元素，再取平均。因此，问题变成求两个有序序列归并后第k小的元素的问题。题目要求对数复杂度，便强行使用二分来解。

二分的评判依据是 `mid` 元素是序列中第几小的元素，若小于k，则继续在右区间查找，大于k则在左区间查找。过程中使用了 `bisect`库，需要注意的是搜 `nums1`元素在 `nums2` 中位置时，使用 `bisect_left`，反之使用 `bisect_right` 。若都使用left或者right会存在找不到正确答案的问题。搜索过程为先1、2同时查找，2若搜完就继续单独搜1，反之亦然。

代码：

```python
import bisect
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        def search_k_min(array1,array2,k):
            l1,r1,l2,r2 = 0, len(array1)-1, 0, len(array2)-1
            while l1<=r1 and l2<=r2:
                mid1 = (l1+r1)//2
                mid2 = (l2+r2)//2
                idx1 = mid1 + bisect.bisect_left(array2, array1[mid1])
                idx2 = mid2 + bisect.bisect_right(array1, array2[mid2])
                if idx1 < k:
                    l1 = mid1 + 1
                elif idx1 > k:
                    r1 = mid1 - 1
                else:
                    return array1[mid1]
                if idx2 < k:
                    l2 = mid2 + 1
                elif idx2 > k:
                    r2 = mid2 - 1
                else:
                    return array2[mid2]
            while l1<=r1:
                mid1 = (l1+r1)//2
                idx1 = mid1 + bisect.bisect_left(array2, array1[mid1])
                if idx1 < k:
                    l1 = mid1 + 1
                elif idx1 > k:
                    r1 = mid1 - 1
                else:
                    return array1[mid1]
            while l2<=r2:
                mid2 = (l2+r2)//2
                idx2 = mid2 + bisect.bisect_right(array1, array2[mid2])
                if idx2 < k:
                    l2 = mid2 + 1
                elif idx2 > k:
                    r2 = mid2 - 1
                else:
                    return array2[mid2]

        
        total_len = len(nums1)+len(nums2)
        median = float(search_k_min(nums1, nums2, total_len//2))
        if total_len % 2 == 1:
            return median
        median += float(search_k_min(nums1, nums2, total_len//2 - 1))
        median /= 2
        return median
```



## 4 排序



## 5 搜索



## 6 动态规划



