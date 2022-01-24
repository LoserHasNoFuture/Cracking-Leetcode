# 855. Exam Room 849
There is an exam room with  `n`  seats in a single row labeled from  `0`  to  `n - 1`.

When a student enters the room, they must sit in the seat that maximizes the distance to the closest person. If there are multiple such seats, they sit in the seat with the lowest number. If no one is in the room, then the student sits at seat number  `0`.

Design a class that simulates the mentioned exam room.

Implement the  `ExamRoom`  class:

-   `ExamRoom(int n)`  Initializes the object of the exam room with the number of the seats  `n`.
-   `int seat()`  Returns the label of the seat at which the next student will set.
-   `void leave(int p)`  Indicates that the student sitting at seat  `p`  will leave the room. It is guaranteed that there will be a student sitting at seat  `p`.

**Example 1:**

**Input**
["ExamRoom", "seat", "seat", "seat", "seat", "leave", "seat"]
[[10], [], [], [], [], [4], []]
**Output**
[null, 0, 9, 4, 2, null, 5]

**Explanation**
ExamRoom examRoom = new ExamRoom(10);
examRoom.seat(); // return 0, no one is in the room, then the student sits at seat number 0.
examRoom.seat(); // return 9, the student sits at the last seat number 9.
examRoom.seat(); // return 4, the student sits at the last seat number 4.
examRoom.seat(); // return 2, the student sits at the last seat number 2.
examRoom.leave(4);
examRoom.seat(); // return 5, the student sits at the last seat number 5.

**Constraints:**

-   `1 <= n <= 109`
-   It is guaranteed that there is a student sitting at seat  `p`.
-   At most  `104`  calls will be made to  `seat`  and  `leave`.

# Solution:
```
class ExamRoom {

    class ListNode{
        int seat;
        ListNode next;
        
        public ListNode(int _seat){
            this.seat = _seat;
            this.next = null;
        }
    }
    
    int capacity, cnt;
    ListNode head;
    public ExamRoom(int n) {
        capacity = n;
        cnt = 0;
        head = new ListNode(-1);
    }

    public int seat() {
        if(cnt == capacity) return -1;
        
        cnt++;
        if(head.next == null) {
            head.next = new ListNode(0);
            return 0;
        }
        ListNode prev = head, cur = head.next;
        int distance = head.next.seat;
        
        while(cur != null){
            distance = Math.max(distance, (cur.seat - prev.seat)/2);
            cur = cur.next;
            prev = prev.next;
        }
        
        distance = Math.max(distance, capacity - 1 - prev.seat);
        prev = head; cur = head.next;
        
        ListNode insert = null;
        if(cur.seat == distance){
            insert = new ListNode(0);
        }else{
            while(cur != null && (cur.seat - prev.seat)/2 != distance){
                prev = prev.next;
                cur = cur.next;
            }
            
            if(cur == null) insert = new ListNode(capacity-1);
            else insert = new ListNode(prev.seat + distance);
        }
        
        prev.next = insert;
        insert.next = cur;
        
        return insert.seat;
    }

    public void leave(int p) {
        ListNode cur = head.next, prev = head;
        
        while(cur != null && cur.seat != p){
            prev = cur;
            cur = cur.next;
        }
        
        if(cur != null){
            prev.next = cur.next;
            cnt--;
        }
    }
}

/**
 * Your ExamRoom object will be instantiated and called as such:
 * ExamRoom obj = new ExamRoom(n);
 * int param_1 = obj.seat();
 * obj.leave(p);
 */
```