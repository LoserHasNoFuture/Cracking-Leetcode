# 475. Heaters

### Weird thing: is house sorted?
Winter is coming! Your first job during the contest is to design a standard heater with fixed warm radius to warm all the houses.

Now, you are given positions of houses and heaters on a horizontal line, find out minimum radius of heaters so that all houses could be covered by those heaters.

So, your input will be the positions of houses and heaters seperately, and your expected output will be the minimum radius standard of heaters.

**Note:**

1.  Numbers of houses and heaters you are given are non-negative and will not exceed 25000.
2.  Positions of houses and heaters you are given are non-negative and will not exceed 10^9.
3.  As long as a house is in the heaters' warm radius range, it can be warmed.
4.  All the heaters follow your radius standard and the warm radius will the same.

**Example 1:**

**Input:** [1,2,3],[2]
**Output:** 1
**Explanation:** The only heater was placed in the position 2, and if we use the radius 1 standard, then all the houses can be warmed.



# Solution 1: QuickSort + Binary Search 
```
public class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        Arrays.sort(heaters);
        int result = Integer.MIN_VALUE;
        
        for (int house : houses) {
            int index = Arrays.binarySearch(heaters, house);
            if (index < 0) {
        	index = -(index + 1);
            }
            int dist1 = index - 1 >= 0 ? house - heaters[index - 1] : Integer.MAX_VALUE;
            int dist2 = index < heaters.length ? heaters[index] - house : Integer.MAX_VALUE;
        
            result = Math.max(result, Math.min(dist1, dist2));
        }
        
        return result;
    }
}
```
My sorting is crappy:
```
class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        quickSort(heaters, 0, heaters.length - 1);
        
        
        int res = 0;
        for(int i = 0; i < houses.length; i++){
            res = Math.max(res,find_closest(heaters,houses[i]));    
        }
        
        return res;
    }
    
    public int find_closest(int[] nums, int target){
        int start = 0;
        int end = nums.length - 1;
        
        int res = Integer.MAX_VALUE;
        while(start < end){
            int mid = (start + end)/2;
            if(nums[mid] == target) return 0;
            if(nums[mid] > target) {
                res = Math.min(nums[mid]-target,res);
                end = mid;
            }
            else {
                res = Math.min(target - nums[mid],res);
                start = mid + 1;
            }
        }
        
        return Math.min(Math.abs(target - nums[start]),res);
        
    }
    
    public void quickSort(int[] nums, int left, int right){
        if(left >= right) return;
        int pivot = left;
        
        int l = left + 1;
        int r = right;
        
        while(l <= r){
            while(l < nums.length && nums[l] < nums[pivot]) l++;
            while(r >= 0 && nums[r] > nums[pivot]) r--;
            
            if(l < r){
                int temp = nums[l];
                nums[l] = nums[r];
                nums[r] = temp;
                l++;
                r--;
            }
        }
        
        
        int temp = nums[pivot];
        nums[pivot] = nums[r];
        nums[r] = temp;
        quickSort(nums, left, r - 1);
        quickSort(nums, r+1, right);

        
    }
}
```


# Solution 2: QuickSort and Two Pointers 
My Sorting is crappy, using Arrays.sort() is a lot better.
```
class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        quickSort(houses, 0, houses.length - 1);
        quickSort(heaters, 0, heaters.length - 1);
        
        int res = 0;
        int p1 = 0;
        int p2 = 0;
        while(p1 < houses.length && p2 < heaters.length){
            int radius = Math.abs(heaters[p2]-houses[p1]);
            while(p2 + 1 < heaters.length){
                int next_radius = Math.abs(heaters[p2+1]-houses[p1]);
                if(next_radius <= radius) {
                    radius = next_radius;
                    p2++;
                }
                else break;
            }
            radius = Math.abs(heaters[p2]-houses[p1]);
            if(radius > res) res = radius;
            p1++;
        }
        
        return res;
    }
    
    
    public void quickSort(int[] nums, int left, int right){
        if(left >= right) return;
        int pivot = left;
        
        int l = left + 1;
        int r = right;
        
        while(l <= r){
            while(l < nums.length && nums[l] <= nums[pivot]) l++;
            while(r >= 0 && nums[r] > nums[pivot]) r--;
            
            if(l < r){
                int temp = nums[l];
                nums[l] = nums[r];
                nums[r] = temp;
                l++;
                r--;
            }
        }
        
        
        int temp = nums[pivot];
        nums[pivot] = nums[r];
        nums[r] = temp;
        quickSort(nums, left, r - 1);
        quickSort(nums, r+1, right);

        
    }
}
```

Others' neat code:
```
public class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        Arrays.sort(houses);
        Arrays.sort(heaters);
        
        int i = 0, j = 0, res = 0;
        while (i < houses.length) {
            while (j < heaters.length - 1
                && Math.abs(heaters[j + 1] - houses[i]) <= Math.abs(heaters[j] - houses[i])) {
                j++;
            }
            res = Math.max(res, Math.abs(heaters[j] - houses[i]));
            i++;
        }
        
        return res;
    }
}
```



