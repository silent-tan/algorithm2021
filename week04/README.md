## 355. 设计推特

### 解题思路
- 用户信息内维护关注
- 推文列表先插入自己的推文
- 队头为新

### 代码

```cpp
struct User {
    // 当前关注
    unordered_set<int> followIds;
    // 双端队列，最新在头部；用户十条推文，time累加表示时间先后
    //         时间   推文id
    deque<pair<int, int>> tweets;
};

class Twitter {
public:
    /** Initialize your data structure here. */
    Twitter() {
        time = 0;
    }
    
    /** Compose a new tweet. */
    void postTweet(int userId, int tweetId) {
        auto& user = id2User[userId];
        // 超过10条出队
        if (user.tweets.size() >= 10) {
            // 最后一个元素出队
            user.tweets.pop_back();
        }
        // 最新推文进入队头
        user.tweets.emplace_front(++time, tweetId);
    }
    
    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    vector<int> getNewsFeed(int userId) {
        auto cmp = [](const pair<int, int>& a, const pair<int, int>& b) {
            return a.first > b.first;
        };

        // 小顶堆: 优先队列
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> q(cmp);
        // 用户的推文
        vector<int> result;
        auto& user = id2User[userId];
        // 插入所有推文
        for(pair<int, int> tweet: user.tweets) {
            q.emplace(tweet);
        }
        // 插入关注着推文
        for(int followId: user.followIds) {
            for(pair<int, int> tweet: id2User[followId].tweets) {
                if (q.size() >= 10) {
                    // 比较新插入的时间和队头推文的时间
                    if (tweet.first > q.top().first) {
                        q.pop();
                        q.emplace(tweet);
                    }
                } else {
                    q.emplace(tweet);
                }
            }
        }

        // 返回用户推文列表
        while(!q.empty()) {
            result.push_back(q.top().second);
            q.pop();
        }
        reverse(result.begin(), result.end());

        return result;
    }
    
    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    void follow(int followId, int followeeId) {
        auto& user = id2User[followId];
        user.followIds.insert(followeeId);
    }
    
    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    void unfollow(int followerId, int followeeId) {
        auto& user = id2User[followerId];
        user.followIds.erase(followeeId);
    }
private:
    unordered_map<int, User> id2User;
    int time; 
};

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter* obj = new Twitter();
 * obj->postTweet(userId,tweetId);
 * vector<int> param_2 = obj->getNewsFeed(userId);
 * obj->follow(followerId,followeeId);
 * obj->unfollow(followerId,followeeId);
 */
```
