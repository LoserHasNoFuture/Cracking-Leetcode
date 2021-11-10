# 503. Next Greater Element II 496 1019
Given a circular integer array  `nums`  (i.e., the next element of  `nums[nums.length - 1]`  is  `nums[0]`), return  _the  **next greater number**  for every element in_  `nums`.

The  **next greater number**  of a number  `x`  is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return  `-1`  for this number.

**Example 1:**

**Input:** nums = [1,2,1]
**Output:** [2,-1,2]
Explanation: The first 1's next greater number is 2; 
The number 2 can't find next greater number. 
The second 1's next greater number needs to search circularly, which is also 2.

**Example 2:**

**Input:** nums = [1,2,3,4,3]
**Output:** [2,3,4,-1,4]

**Constraints:**

-   `1 <= nums.length <= 104`
-   `-109  <= nums[i] <= 109`

# Solution 1: Loop Twice (Beat 97%)
```
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        Deque<Integer> stack = new ArrayDeque<Integer>();
        int[] res = new int[nums.length];
        Arrays.fill(res,-1);
        for(int i = 0; i < nums.length*2; i++){
            int index = i%nums.length;
            int val = nums[index];
            
            while(!stack.isEmpty() && nums[stack.peek()] < val){
                int tmp = stack.pop();
                if(res[tmp] != -1) continue;
                res[tmp] = val;
            }
            
            stack.push(index);
        }
        
        return res;
    }
}
```

# Solution 2: From Discussion Section (Beat 100%)
I don't think I can master this method.
```
public class Solution {
    public int[] nextGreaterElements(int[] nums) {

        //Case when array is empty
        if(nums.length == 0) return nums;
      
        int[] result = new int[nums.length];

        //Assuming array to be non-cyclical, last element does not have next larger element
        result[nums.length - 1] = -1;

        //Case when only one element is there in array     
        if(result.length == 1) return result;

        for (int i = nums.length - 2; i >= 0; i--){   
            //Starting from next element
            int k = i + 1;

            //Keep tracking next larger element until you find it or you find element with no next larger element
            while(nums[i] >= nums[k] && result[k] != -1){
                k = result[k];
            }
            
            //If next larger element is found, store index      
            if(nums[k] > nums[i]) result[i] = k;
            //If not found, it doesn't have next larger element
            else result[i] = -1;
        }
        
        //Second iteration assuming cyclical array, last element can also have next larger element
        for (int i = nums.length - 1; i >= 0; i--){   

            //If next larger element assuming non-cyclical array already exists, continue
            if(result[i] != -1) continue;
      
            //Case when i + 1 is greater than length of array: when on last element
            int k = (i + 1) % nums.length;

            //Keep tracking next larger element until you find it or you find element with no next larger element
            while(nums[i] >= nums[k] && result[k] != -1){
                k = result[k];
            }

            //If next larger element is found, store it's index
            if(nums[k] > nums[i]) result[i] = k;
            //If not found, it doesn't have next larger element
            else result[i] = -1;
        }

        //Replace indices with actual values
        for(int i = 0; i < nums.length; i++){
            if(result[i] != -1) result[i] = nums[result[i]];
        }

        return result;
    }
}
```