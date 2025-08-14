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

**4. Cache Memory**
The amount of cache required is 1% of daily storage  
Amount of cache memory = 0.01 * 216TB = 2.16TB  

**5. Network/Bandwith**
- Data flowing into out system per second is also known as Ingress
- Data flowing out of our system persecond is also known as Egress

****Ingress**** = Data comming in a day / seconds in a day  
        = 216TB / (24*60*60)
        = 2.5GB/sec
Note 216TB we have calculated above  

Average size of the post = (0.2 * 100KB) + (0.6*0.5MB) + (0.2*20MB) (This calculation based on the assumptions in the storage sections)  
                         = 4.32MB  
The amount of data going out in a day = no.of read requests per day * avg size of post  
                                      = 50B * 4.32MB  
                                      = 216PB / Day  
****Egress**** = the amount of data going out in a day / seconds in a day  
       = 216PB / (24*60*60)  
       = 2.5TB/sec  

### API Design  
**Creating Text Post:**  
HTTP Method: POST  
Endpoint: /v1/posts  
HTTP Body:  
{  
  userId: "1234",  
  text: "Trip to India"  
  hashtags: ["travel", "fun"]  
}   
  
**Creating Image/Video Post:**  
HTTP Method: POST  
Endpoint: /v1/posts  
HTTP Body:  
{  
  userId: "123456",  
  mediaUrl: "link",  
  descritption: "Relaxing by the beach."  
  hashing: ["relax", "chilling"]  
}  
Note: before we sending the above request we have to store the image/video in the object storage, Once its stored it will returns its url. this url will be used in the above http body as mediaurl.  

**Commenting / liking:**  
HTTP Method: POST  
Endpoint:  /v1/comments
HTTP Body:  
{  
  userId: "12345",  
  postId: "2345",   
  comment: "Beautiful, great shot!"  
}  
HTTP Method: POST  
Endpoint:  /v1/like
HTTP Body:  
{  
  userId: "12345",  
  postId: "2345",    
}  

**Follow/Unfollow:**  
HTTP Method: POST  
Endpoint: /v1/follow  
HTTP Body:  
{
  followerId: "1234",  
  followeeId: "12345"  
}  

Reading Posts:  
HTTP Method: GET  
Endpoint: /v1/feeds/{userId}  

### High Level Design

**High Level Design for follow/unfollow:**  
Client -> API Gate way ->Load Balancer -> follow/unfollow service -> Follow DB(Graph DB) -> Response back  
  
1. client makes an api call for follow/unfollow.
2. API gateway validates the request and send it to LB.
3. Load balancer manages the traffic on the servers. It will transerfer the request to availabe servers.
4. server process the request and store the follow/unfollow data in the Graph DB.
5. It sends responsed via same way back to client.

****Graph DB****: It stores and manages the user relationships data as nodes and edges and optimized for traversals like get all followers of user X or mutual connections.  

**High Level Design for Text post**  
Creating text post:  
Clinet -> API Gateway -> Load Balancer -> post writter service -> Posts DB  

Reading text post:  
User feed shows the posts from the users that they follow in a reverse chronological order  
  
Client -> API Gateway -> Load Balancer -> NewsFeed Reader Service -> Follow DB -> NewsFeed Read Service -> Posts DB -> response back to user  

1. News feed reader service gets the all the followee data from the follow db
2. Next news feed service gets the all post of followees from the posts db
3. Newsfeed service arranges them in chronological order and sends it to the user as a response  
For every user the system need to perform above operations eachtime when then open the news feed. this can make the readin newsfeed very very slow or if we increase the servers to make it fast it will increase the cost.

**How to optimize our design:**  
Creating a news feed upfornt for the users. Now user get the news feed directly  without perform above steps. It will make our system higly available  

Creating text post design will be same but it will trigger the generate feed service when it gets the new post.  

Post writter service -> message Queue -> Load balancer -> Generate New Feed service ->  
Generator service -> Posts DB  
Generator service -> Follow DB  
Generator service -> Feeds DB  
Generator service -> feeds cache  

1. Once new post came, the post writter service adds new job with {post Id, userId} to Queue.
2. The new post waits in the queue till its turn come up.
3. The job will be moved to procssing to generator service.
4. Generator service first gets the new post usign post id from posts DB.
5. Next generator service get the all the followers of the user from  follow DB using user id.
6. Its addeds this new post to all his/her followers in the feeds DB.
7. Then addeds same thing in the feeds cache for speed access.

Now Read service gets the feed from cache or feedsDB  

**High Level Design For Video/Image Post**  
The video or image post is performed in two steps.  
In frist step video or image needs to added to the object storage and get the pre signed url.
seconde step is same like the text post  

Step 1 High level design:  
client -> API gateway -> load balancer -> Pre signed url generator -> object storage -> send pre signed url back to client  
  
Step 2 High level design same as the text post design  
