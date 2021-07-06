# 1472. Design Browser History
You have a  **browser**  of one tab where you start on the  `homepage`  and you can visit another  `url`, get back in the history number of  `steps`  or move forward in the history number of  `steps`.

Implement the  `BrowserHistory`  class:

-   `BrowserHistory(string homepage)`  Initializes the object with the  `homepage` of the browser.
-   `void visit(string url)` Visits `url`  from the current page. It clears up all the forward history.
-   `string back(int steps)` Move  `steps`  back in history. If you can only return  `x`  steps in the history and  `steps > x`, you will return only  `x`  steps. Return the current  `url` after moving back in history  **at most**  `steps`.
-   `string forward(int steps)` Move  `steps`  forward in history. If you can only forward  `x`  steps in the history and  `steps > x`, you will forward only `x`  steps. Return the current  `url` after forwarding in history  **at most**  `steps`.

**Example:**

**Input:**
["BrowserHistory","visit","visit","visit","back","back","forward","visit","forward","back","back"]
[["leetcode.com"],["google.com"],["facebook.com"],["youtube.com"],[1],[1],[1],["linkedin.com"],[2],[2],[7]]
**Output:**
[null,null,null,null,"facebook.com","google.com","facebook.com",null,"linkedin.com","google.com","leetcode.com"]

**Explanation:**
```
BrowserHistory browserHistory = new BrowserHistory("leetcode.com");
browserHistory.visit("google.com");       // You are in "leetcode.com". Visit "google.com"
browserHistory.visit("facebook.com");     // You are in "google.com". Visit "facebook.com"
browserHistory.visit("youtube.com");      // You are in "facebook.com". Visit "youtube.com"
browserHistory.back(1);                   // You are in "youtube.com", move back to "facebook.com" return "facebook.com"
browserHistory.back(1);                   // You are in "facebook.com", move back to "google.com" return "google.com"
browserHistory.forward(1);                // You are in "google.com", move forward to "facebook.com" return "facebook.com"
browserHistory.visit("linkedin.com");     // You are in "facebook.com". Visit "linkedin.com"
browserHistory.forward(2);                // You are in "linkedin.com", you cannot move forward any steps.
browserHistory.back(2);                   // You are in "linkedin.com", move back two steps to "facebook.com" then to "google.com". return "google.com"
browserHistory.back(7);                   // You are in "google.com", you can move back only one step to "leetcode.com". return "leetcode.com"
```

**Constraints:**

-   `1 <= homepage.length <= 20`
-   `1 <= url.length <= 20`
-   `1 <= steps <= 100`
-   `homepage`  and  `url`  consist of '.' or lower case English letters.
-   At most  `5000` calls will be made to  `visit`,  `back`, and  `forward`.


# Solution 1: Doublely Linked List (Beat 90%)
```
class BrowserHistory {

    class ListNode{
        String url;
        ListNode next;
        ListNode prev;
        
        public ListNode(String _url){
            this.url = _url;
        }
    }
    
    ListNode cur;
    public BrowserHistory(String homepage) {
        cur = new ListNode(homepage);
    }
    
    public void visit(String url) {
        ListNode next = new ListNode(url);
        cur.next = next;
        next.prev = cur;
        cur = next;
    }
    
    public String back(int steps) {
        while(cur.prev != null && steps-- > 0) cur = cur.prev;
        
        return cur.url;
    }
    
    public String forward(int steps) {
        while(cur.next != null && steps-- > 0) cur = cur.next;
        
        return cur.url;
    }
}

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory obj = new BrowserHistory(homepage);
 * obj.visit(url);
 * String param_2 = obj.back(steps);
 * String param_3 = obj.forward(steps);
 */
```

# Solution 2: Using Stack (Beat 35%)
Refer From: [https://leetcode.com/problems/design-browser-history/discuss/674486/Two-Stacks-Pretty-code.](https://leetcode.com/problems/design-browser-history/discuss/674486/Two-Stacks-Pretty-code.)
```
class BrowserHistory {
    
    Stack<String> history = new Stack<>();
    Stack<String> future = new Stack<>();
    public BrowserHistory(String homepage) {
        history.add(homepage);
        future.clear();
    }
    
    public void visit(String url) {
        history.add(url);
        future.clear();
    }
    
    public String back(int steps) {
        while (steps > 0 && history.size() > 1) {
            future.add(history.pop());
            steps--;
        }
        return history.peek();
    }
    
    public String forward(int steps) {
        while (steps > 0 && future.size() > 0) {
            history.add(future.pop());
            steps--;
        }
        return history.peek();
    }
}
```