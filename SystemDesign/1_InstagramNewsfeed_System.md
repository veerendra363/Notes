# Designing Instagram Newsfeed

### Functional Requirements
1. Creating Post: User can Create social media post. (It be will text, image or video)
2. Follow / Unfollow another user
3. News Feed: User can view news feed (The feed shows the posts from the users that they follow in a reverse chronological order)
4. Like/Comment: Users can like or comment on other people's post.
5. User Notification: users get notified when another user likes or comments on their post.

### Non Functional Requirements
1. Availability - The system shoudl be highly aavilabe - 99.999% uptime.  
2. Eventual Consistency - If a user posts something, it's okay if it doesn't show up immediately but apperas within a few seconds(1-2 secs).  
3. Latency - If we lick on home button newsfeed should load in 1-2 seconds.  
4. Scalability - The system should scale throught the globe.
5. Extensibilitiy -  Easier to extend it in the future. if we need to ad features like replying to a comment, post recommendations, or ads
6. Usability - For newsfeed system rendering should be super fast.

### Capacity Estimation
Why Capacity estimations?  
- Determine no of servers and databases
- Cost management
- Decide type and specifications of all hardware
- Helps us determine is it read heavy or write heary system

**1. DAU**  
Lets assume we have 500 million daily active users  

**2. Throughput**  
***Write Throughput***  
According to functional requirements there are 3 possible ways in which we write to our system.(creating post, following, liking/commenting)  
Lets assume  
1. 10% of DAU will post each day.
2. Each user follows/unfollows 1 new user every week.
3. Each user performs 3 activities(likes/comments) every day.
  
No.of post requests = 10% of 500M = 50M write requests per day  
No.of follow/unfollow requests = 500M / 7 = 71.4M write requests per day  
No.of like/comment requests = 500M *3 = 1.5B Write requests per day  
Total write requests per day  
= No.of post reuests + No.of follow/unfollow reqeusts + No.of like/comment requests  
= 500M + 71.4M + 1.5B  
= 2.071B Write Requests Per day


  
***Read Throughput***  
According to fucntional requirement there is only 1 posssible way to read(view post/newsfeed)  
Lets assume each person opens our system 10 time days  and each time user view 10 postes  
Total read requests = DAU * No.of times opens * No.of postes viewed each time  
                    = 500M * 10 * 10  
                    = 50 Billion read requests per day  

**3. Storage**  
According to functional requirements we are going to store the post, following, likes, and comments.  
There are three kind of posts - video,  image, text  
Lets assume
1. avg size of video post is 20 MB and 20% of DAU post videos
2. avg size of image post is 0.5MB and 60% of DAU post images
3. avg size of text post is 100KB and 20% of DAU post text
4. follow data 16 bytes per follow
5. Activity data(like/commnet) 216 bytes per activity

Storage per post per day = 0.2 * 50M * 20MB + 0.6 * 50M * 0.5MB + 0.2 * 50M * 100KB   
                         = 200TB + 15TB + 1TB  
                         = 216TB per day  
Storage per following per day = 71.4M * 16bytes   
                              = 1.15 GB per day  
Storage per activity(like / comment) = 1.5B *216 bytes  
                                     = 300 GB per day  
Total required storage required per day = 216TB + 1.15GB + 300GB  
                                        = 216.3 TB per day  

Storage per 10 years = 216.3 TB * 365 * 10 = 720 PB
