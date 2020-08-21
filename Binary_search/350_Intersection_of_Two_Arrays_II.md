# 350. Intersection of Two Arrays II (Similar to 349)
Given two arrays, write a function to compute their intersection.

**Example 1:**

**Input:** nums1 = [1,2,2,1], nums2 = [2,2]
**Output:** [2,2]

**Example 2:**

**Input:** nums1 = [4,9,5], nums2 = [9,4,9,8,4]
**Output:** [4,9]

**Note:**

-   Each element in the result should appear as many times as it shows in both arrays.
-   The result can be in any order.

**Follow up:**

-   What if the given array is already sorted? How would you optimize your algorithm?
-   What if  _nums1_'s size is small compared to  _nums2_'s size? Which algorithm is better?
-   What if elements of  _nums2_  are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

# Solution 1: HashMap
```
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if(nums1.length == 0 || nums2.length == 0) return new int[0];
        ArrayList<Integer> resl = new ArrayList<>();
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        
        for(int i = 0; i < nums1.length; i++) map.put(nums1[i], map.getOrDefault(nums1[i],0)+1);
        
        for(int j = 0; j < nums2.length; j++){
            if(map.getOrDefault(nums2[j],0) > 0){
                resl.add(nums2[j]);
                map.put(nums2[j], map.getOrDefault(nums2[j],0) - 1);
            }
        }
        
        int[] res = new int[resl.size()];
        for(int i = 0; i < res.length; i++) res[i] = resl.get(i);
        return res;
    }
}
```

# Solution 2: Sorting and Using Two Pointers
Without binary search:
```
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if(nums1.length == 0 || nums2.length == 0) return new int[0];
        ArrayList<Integer> resl = new ArrayList<>();
            
        quickSort(nums1,0,nums1.length-1);
        quickSort(nums2,0,nums2.length-1);
        
        int p1 = 0, p2 = 0;
        while(p1 < nums1.length && p2 < nums2.length){
            if(nums1[p1] < nums2[p2]) p1++;
            else if(nums1[p1] > nums2[p2]) p2++;
            else{
            // compared with 349, only needs to change this line
                resl.add(nums1[p1]);
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
    public int[] intersect(int[] nums1, int[] nums2) {
        if(nums1.length == 0 || nums2.length == 0) return new int[0];
        ArrayList<Integer> resl = new ArrayList<>();
            
        quickSort(nums1,0,nums1.length-1);
        quickSort(nums2,0,nums2.length-1);
              
        int p1 = 0, p2 = 0;
        while(p1 < nums1.length && p2 < nums2.length){
            if(nums1[p1] < nums2[p2]) p1 = find_next(nums1, p1+1,nums2[p2]);
            else if(nums1[p1] > nums2[p2]) p2 = find_next(nums2, p2+1, nums1[p1]);
            else{
            // similarly, here is also changed compared with 349
                resl.add(nums1[p1]);
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

        while(start < end){
            int mid = (start + end)/2;
            if(nums[mid] < target) {
                start = mid + 1;
                if(nums[start] >= target) return start;
            }
            else if(nums[mid] == target){
            // compare with 349, here is changed
                while(nums[mid] == target) mid--;
                return mid+1;
            } 
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