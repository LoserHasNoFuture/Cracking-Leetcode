
# 349. Intersection of Two Arrays (Similar to 350)
Given two arrays, write a function to compute their intersection.

**Example 1:**

**Input:** nums1 = [1,2,2,1], nums2 = [2,2]
**Output:** [2]

**Example 2:**

**Input:** nums1 = [4,9,5], nums2 = [9,4,9,8,4]
**Output:** [9,4]

**Note:**

-   Each element in the result must be unique.
-   The result can be in any order.

# Solution 1: HashSet

```
public class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set = new HashSet<>();
        Set<Integer> intersect = new HashSet<>();
        for (int i = 0; i < nums1.length; i++) {
            set.add(nums1[i]);
        }
        for (int i = 0; i < nums2.length; i++) {
            if (set.contains(nums2[i])) {
                intersect.add(nums2[i]);
            }
        }
        int[] result = new int[intersect.size()];
        int i = 0;
        for (Integer num : intersect) {
            result[i++] = num;
        }
        return result;
    }
}
```

# Solution 2: Sorting and Using Two pointers
Without binary search:
```
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if(nums1.length == 0 || nums2.length == 0) return new int[0];
        ArrayList<Integer> resl = new ArrayList<>();
            
        quickSort(nums1,0,nums1.length-1);
        quickSort(nums2,0,nums2.length-1);
        
        int p1 = 0, p2 = 0;
        while(p1 < nums1.length && p2 < nums2.length){
            if(nums1[p1] < nums2[p2]) p1++;
            else if(nums1[p1] > nums2[p2]) p2++;
            else{
                if (resl.size() == 0 || resl.get(resl.size()-1) != nums1[p1]) resl.add(nums1[p1]);
                p1++;
                p2++;
            }
        }
        
        int[] res = new int[resl.size()];
        for(int i = 0; i < res.length; i++) res[i] = resl.get(i);
        return res;
    }
    
    public void quickSort(int[] nums, int l, int r){
        if(l >= r) return;
        
        int pivot = l;
        int left = l + 1;
        int right = r;
        
        while(left <= right){
            while(left <= r && nums[left] <= nums[pivot]) left++;
            while(right >= l && nums[right] > nums[pivot]) right--;
            
            if(left < right){
                int temp = nums[left];
                nums[left] = nums[right];
                nums[right] = temp;
                left++;right--;
            }   
        }
        
        int temp = nums[pivot];
        nums[pivot] = nums[right];
        nums[right] = temp;
        quickSort(nums, right + 1, r);
        quickSort(nums, l, right - 1);
    }
}
```

With binary search:
```
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if(nums1.length == 0 || nums2.length == 0) return new int[0];
        ArrayList<Integer> resl = new ArrayList<>();
            
        quickSort(nums1,0,nums1.length-1);
        quickSort(nums2,0,nums2.length-1);
              
        int p1 = 0, p2 = 0;
        while(p1 < nums1.length && p2 < nums2.length){
            if(nums1[p1] < nums2[p2]) p1 = find_next(nums1, p1+1,nums2[p2]);
            else if(nums1[p1] > nums2[p2]) p2 = find_next(nums2, p2+1, nums1[p1]);
            else{
                if (resl.size() == 0 || resl.get(resl.size()-1) != nums1[p1]) resl.add(nums1[p1]);
                p1++;
                p2++;
            }
        }
        
        int[] res = new int[resl.size()];
        for(int i = 0; i < res.length; i++) res[i] = resl.get(i);
        return res;
    }
    
    public int find_next(int[] nums, int start ,int target){
        int end = nums.length - 1;
        if(start > end) return start;
        
        while(start < end){
            int mid = (start + end)/2;
            if(nums[mid] < target) {
                start = mid + 1;
                if(nums[start] >= target) return start;
            }
            else if(nums[mid] == target) return mid;
            else end = mid;
        }
        
        return start;
    }
    
    public void quickSort(int[] nums, int l, int r){
        if(l >= r) return;
        
        int pivot = l;
        int left = l + 1;
        int right = r;
        
        while(left <= right){
            while(left <= r && nums[left] <= nums[pivot]) left++;
            while(right >= l && nums[right] > nums[pivot]) right--;
            
            if(left < right){
                int temp = nums[left];
                nums[left] = nums[right];
                nums[right] = temp;
                left++;right--;
            }   
        }
        
        int temp = nums[pivot];
        nums[pivot] = nums[right];
        nums[right] = temp;
        quickSort(nums, right + 1, r);
        quickSort(nums, l, right - 1);   
    }
}
```