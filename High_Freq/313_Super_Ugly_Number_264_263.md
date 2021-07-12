# 313. Super Ugly Number 264 263
A  **super ugly number**  is a positive integer whose prime factors are in the array  `primes`.

Given an integer  `n`  and an array of integers  `primes`, return  _the_  `nth`  _**super ugly number**_.

The  `nth`  **super ugly number**  is  **guaranteed**  to fit in a  **32-bit**  signed integer.

**Example 1:**

**Input:** n = 12, primes = [2,7,13,19]
**Output:** 32
**Explanation:** [1,2,4,7,8,13,14,16,19,26,28,32] is the sequence of the first 12 super ugly numbers given primes = [2,7,13,19].

**Example 2:**

**Input:** n = 1, primes = [2,3,5]
**Output:** 1
**Explanation:** 1 has no prime factors, therefore all of its prime factors are in the array primes = [2,3,5].

**Constraints:**

-   `1 <= n <= 106`
-   `1 <= primes.length <= 100`
-   `2 <= primes[i] <= 1000`
-   `primes[i]`  is  **guaranteed**  to be a prime number.
-   All the values of  `primes`  are  **unique**  and sorted in  **ascending order**.

# Solution 1: Similar to 264 (O(kn), Beat 85%)
```
class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        int[] index = new int[primes.length];
        int[] tmp_res = new int[primes.length];
        int[] dp = new int[n+1];
        dp[1] = 1;
        Arrays.fill(index,1);
        
        for(int i = 0; i < primes.length; i++) tmp_res[i] = primes[i];
        
        for(int i = 2; i <= n; i++){
            int min = find_min(tmp_res);
            dp[i] = min;
            for(int j = 0; j < tmp_res.length; j++){
                if(tmp_res[j] == min) tmp_res[j] = dp[++index[j]] * primes[j];
            }
        }
        
        return dp[n];
    }
    
    public int find_min(int[] nums){
        int min = Integer.MAX_VALUE;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] < min){
                min = nums[i];
            }
        }
        return min;
    }
}
```

#  Solution 2: Priority Queue (Worst Case  O(kn*logk), Beat 30%)
```
class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        int[] index = new int[primes.length];
        int[] dp = new int[n+1];
        dp[1] = 1;
        Arrays.fill(index,1);
        
        PriorityQueue<Node> pq = new PriorityQueue<Node>();
        for(int prime: primes) pq.offer(new Node(1,prime,dp[1]*prime));
        
//      O(n*logk*k)
        for(int i = 2; i <= n; i++){
            
            while(dp[i-1] == pq.peek().val){
                Node waste = pq.poll();
                pq.offer(new Node(waste.index+1,waste.prime,waste.prime*dp[waste.index+1]));
            }
            
            Node cur = pq.poll();
            dp[i] = cur.val;
            pq.offer(new Node(cur.index+1,cur.prime,cur.prime*dp[cur.index+1]));
        }
        
        return dp[n];
    }
    
    class Node implements Comparable<Node>{
        int index;
        int prime;
        int val;
        
        public Node(int _index, int _prime, int _val){
            this.index = _index;
            this.prime = _prime;
            this.val = _val;
        }
        
        @Override
        public int compareTo(Node that){
            return this.val - that.val;
        }
    }
}
```