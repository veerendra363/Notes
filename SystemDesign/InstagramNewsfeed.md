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
