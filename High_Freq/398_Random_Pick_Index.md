# 398. Random Pick Index
Given an integer array  `nums`  with possible  **duplicates**, randomly output the index of a given  `target`  number. You can assume that the given target number must exist in the array.

Implement the  `Solution`  class:

-   `Solution(int[] nums)`  Initializes the object with the array  `nums`.
-   `int pick(int target)`  Picks a random index  `i`  from  `nums`  where  `nums[i] == target`. If there are multiple valid i's, then each index should have an equal probability of returning.

**Example 1:**

**Input**
["Solution", "pick", "pick", "pick"]
[[[1, 2, 3, 3, 3]], [3], [1], [3]]
**Output**
[null, 4, 0, 2]

**Explanation**
Solution solution = new Solution([1, 2, 3, 3, 3]);
solution.pick(3); // It should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
solution.pick(1); // It should return 0. Since in the array only nums[0] is equal to 1.
solution.pick(3); // It should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.

**Constraints:**

-   `1 <= nums.length <= 2 * 104`
-   `-231  <= nums[i] <= 231  - 1`
-   `target`  is an integer from  `nums`.
-   At most  `104`  calls will be made to  `pick`.


**Three kinds of solution:**
Refer from: [https://leetcode.com/problems/random-pick-index/discuss/88080/What-on-earth-is-meant-by-too-much-memory](https://leetcode.com/problems/random-pick-index/discuss/88080/What-on-earth-is-meant-by-too-much-memory)
Because I've made a rather naive map-of-index-lists Java solution and it was happily accepted by the OJ. So far I see three types of solutions:

1.  Like mine, O(N) memory, O(N) init, O(1) pick.
    
2.  Like @dettier's  [Reservoir Sampling](https://discuss.leetcode.com/topic/58301/simple-reservoir-sampling-solution). O(1) init, O(1) memory, but O(N) to pick.
    
3.  Like @chin-heng's  [binary search](https://discuss.leetcode.com/topic/58295/share-my-c-solution-o-lg-n-to-pick-o-nlg-n-for-sorting): O(N) memory, O(N lg N) init, O(lg N) pick.
    

Are all three kinds acceptable?

# Solution 1: HashMap + LinkedList (O(1) to pick)
```
class Solution {

    class ListNode {
        int val;
        ListNode next;
        ListNode(int _index){
            this.val = _index;
        }
    }
    
    HashMap<Integer,ListNode> map; 
    Random r;
    public Solution(int[] nums) {
        map = new HashMap<Integer,ListNode>();
        r = new Random();
        for(int i = 0; i < nums.length; i++){
            int num = nums[i];
            ListNode node = new ListNode(i);
            if(map.containsKey(num)){
                ListNode old_head = map.get(num);
                node.next = old_head;
            }
            map.put(num,node);
        }
    }
    
    public int pick(int target) {
        ListNode head = map.get(target);
        int cnt = 1;
        int res = -1;
        while(head != null){
            if(r.nextInt(cnt++) == 0) res = head.val;
            head = head.next;
        }
        return res;
    }
}
```

