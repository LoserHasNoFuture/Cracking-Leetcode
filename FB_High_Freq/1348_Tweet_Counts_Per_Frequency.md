# 1348. Tweet Counts Per Frequency
A social media company is trying to monitor activity on their site by analyzing the number of tweets that occur in select periods of time. These periods can be partitioned into smaller  **time chunks**  based on a certain frequency (every  **minute**,  **hour**, or  **day**).

For example, the period  `[10, 10000]`  (in  **seconds**) would be partitioned into the following  **time chunks**  with these frequencies:

-   Every  **minute**  (60-second chunks):  `[10,69]`,  `[70,129]`,  `[130,189]`,  `...`,  `[9970,10000]`
-   Every  **hour**  (3600-second chunks):  `[10,3609]`,  `[3610,7209]`,  `[7210,10000]`
-   Every  **day**  (86400-second chunks):  `[10,10000]`

Notice that the last chunk may be shorter than the specified frequency's chunk size and will always end with the end time of the period (`10000`  in the above example).

Design and implement an API to help the company with their analysis.

Implement the  `TweetCounts`  class:

-   `TweetCounts()`  Initializes the  `TweetCounts`  object.
-   `void recordTweet(String tweetName, int time)`  Stores the  `tweetName`  at the recorded  `time`  (in  **seconds**).
-   `List<Integer> getTweetCountsPerFrequency(String freq, String tweetName, int startTime, int endTime)`  Returns a list of integers representing the number of tweets with  `tweetName`  in each  **time chunk**  for the given period of time  `[startTime, endTime]`  (in  **seconds**) and frequency  `freq`.
    -   `freq`  is one of  `"minute"`,  `"hour"`, or  `"day"`  representing a frequency of every  **minute**,  **hour**, or  **day**  respectively.

**Example:**

**Input**
["TweetCounts","recordTweet","recordTweet","recordTweet","getTweetCountsPerFrequency","getTweetCountsPerFrequency","recordTweet","getTweetCountsPerFrequency"]
[[],["tweet3",0],["tweet3",60],["tweet3",10],["minute","tweet3",0,59],["minute","tweet3",0,60],["tweet3",120],["hour","tweet3",0,210]]

**Output**
[null,null,null,null,[2],[2,1],null,[4]]

**Explanation**
TweetCounts tweetCounts = new TweetCounts();
tweetCounts.recordTweet("tweet3", 0);                              // New tweet "tweet3" at time 0
tweetCounts.recordTweet("tweet3", 60);                             // New tweet "tweet3" at time 60
tweetCounts.recordTweet("tweet3", 10);                             // New tweet "tweet3" at time 10
tweetCounts.getTweetCountsPerFrequency("minute", "tweet3", 0, 59); // return [2]; chunk [0,59] had 2 tweets
tweetCounts.getTweetCountsPerFrequency("minute", "tweet3", 0, 60); // return [2,1]; chunk [0,59] had 2 tweets, chunk [60,60] had 1 tweet
tweetCounts.recordTweet("tweet3", 120);                            // New tweet "tweet3" at time 120
tweetCounts.getTweetCountsPerFrequency("hour", "tweet3", 0, 210);  // return [4]; chunk [0,210] had 4 tweets

**Constraints:**

-   `0 <= time, startTime, endTime <= 109`
-   `0 <= endTime - startTime <= 104`
-   There will be at most  `104`  calls  **in total**  to  `recordTweet`  and  `getTweetCountsPerFrequency`.


# Solution: HashMap + Binary Search Tree (Beat 100%)
```
class TweetCounts {

    class TreeNode{
        TreeNode left;
        TreeNode right;
        int time;
        int count;
        TreeNode(int _time, int _count ){
            this.time = _time;
            this.count = _count;
        }
    }
    
    HashMap<String,TreeNode> map;
    public TweetCounts() {
        map = new HashMap<String,TreeNode>();
    }
    
    public void recordTweet(String tweetName, int time) {
        if(map.containsKey(tweetName)){
            TreeNode root = map.get(tweetName);
            insert_to_tree(root,time);
        }else{
            map.put(tweetName, new TreeNode(time,1));
        }
    }
    
    public void insert_to_tree(TreeNode root, int time){
        if(root.time > time) {
            if(root.left == null) root.left = new TreeNode(time,1);
            else insert_to_tree(root.left,time);
        }else if(root.time < time){
            if(root.right == null) root.right = new TreeNode(time,1);
            else insert_to_tree(root.right,time);
        }else {
            root.count++;
        }
    }
    
    
    public List<Integer> getTweetCountsPerFrequency(String freq, String tweetName, int startTime, int endTime) {
        List<Integer> res = new ArrayList<>();
        if(!map.containsKey(tweetName)) return res;
        TreeNode root = map.get(tweetName);
        
        
        if(freq.equals("minute")){
            count_tweets(res,startTime,endTime,59,root);
        }else if(freq.equals("hour")){
            count_tweets(res,startTime,endTime,3599,root);
        }else{
            count_tweets(res,startTime,endTime,86399,root);
        }
        return res;
    }
    
    public void count_tweets(List<Integer> res, int startTime, int endTime, int interval, TreeNode root){
        ArrayList<int[]> arr = new ArrayList<int[]>();
        // System.out.println("******");
        find_all_tweets_in_duration(root,startTime,endTime,arr);
        
        
        int index = 0;
        for(int cur = startTime; cur <= endTime; cur = cur + interval + 1){
            int end = cur + interval;
            int tmp = index;
            int cnt = 0;
            while(index < arr.size() && arr.get(index)[0] <= end) {
                cnt += arr.get(index)[1];
                index++;
            }
            res.add(cnt);
        }
        
    }
    
    public void find_all_tweets_in_duration(TreeNode root, int startTime, int endTime, ArrayList<int[]> arr){
        if(root == null) return;
        if(root.time > startTime) find_all_tweets_in_duration(root.left,startTime,endTime,arr);
        if(root.time >= startTime && root.time <= endTime){
            arr.add(new int[]{root.time,root.count});
            // System.out.println(root.time+ ", "+root.count);
        }
        if(root.time < endTime) find_all_tweets_in_duration(root.right,startTime,endTime,arr);
    }
}
```