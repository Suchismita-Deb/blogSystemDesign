## Video sharing and streaming platform.

The design mainly focus on the video upload and streaming part.
### Functional Requirements
- User can upload the video.
- User can stream the video.

Mention the out of the scope to not have confusion. To know what the design will be focused onthe topic.

User view information about the video like the like and comment counts. (Like comment count type design will be helpful)    
User search the video. (Top K videos design)  
User comment the video.  
Recommendation of videos.
User make a channel and manage and subscribe the channel.

### Non-functional Requirements.   

Availability>> Consistency.  
Upload and streaming large videos like 10s GB.   
Low latency streaming with videos in low bandwidth.   
The system to scale high number of videos upload nd watch per day (~1M video uploaded daily and 100M videos watched daily).   
The system should support resumable uploads.    

The out of the scope topic.  
The system should protect bad content in videos.  
The system should protect against bots and fake account upload or consuming videos.    
Monitoring or alerting.

The functional requirement is small the non-functional requirement makes the system interesting.

> Plan the approach.
> Design one be one completing the functional requirement in high level and then move to non-functional part to go deep dives.



