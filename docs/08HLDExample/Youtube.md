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

### Core Entities and API.


No need to get the entire details of the entities get the overview. The update should be clear like now its just outline te entity and API and later will come back and update. The primary entity User, Video and VideoMetadata.

Define the API it will help in guiding the design. The target to define the endpoint for every requirement.

Upload the video - `POST /upload` Request - `{video, videoMetaData}`

Stream the video - `GET /stream/{videoId}` Request - `{videoId, videoMetaData}`

### High-Level Design.

Things to get understanding with the video streaming ad storage topic. 

Video Codec - It compresses and decompresses digital video and make it easier for storage and transmission. Preserves the quality and reduce the size. Codec refers to "encoder/decoder" There are trade-offs like time needed to compress, it needs to support all platform, compression efficiency and quality. Example - H.264, H.265, VP9, AV1.

Video Container - It is a file format that contains the video, audio and metadata. It is a wrapper for the video and audio streams. Example - MP4, MKV, AVI, MOV. 

Bitrate - The bitrate is the number of bits transmitted over a period of time. It is measured in kilobits per sec (kbps) or Mbps.  
The size and quality of the video affects the bitrate. High resolution video with higher framerates (fps) will have higher bitrates. More data needs to be transferred to play the video.  
Compression via codec effect the bitrate - it means more efficient compression lead to larger video being compressed to a smaller size prior to transmission.

Manifest Files - It is the text based documents has the details about the video streams and they are 2 types - primary and media files.  
Primary file - It include all the versions of the video (formats). It is the "root" file and points to media manifest files, representing a different version of the video. The video version is split into small segments like sec long.   
Media manifest fies has the list of the links to the clip files and used by video players to stream video by serving "index" to the segments.

#### User upload the video.

Things to consider - Where to store the video metadata(name, description)?  
Where to store the video data(frame, audio)?   
What do we store for video data?

Video upload rate 1M videos/day. The year there will 365< records in the db. We need to store the video metadata in the db that can be horizontally partitioned like Cassandra.   

Cassandra offers high availability and the videoId will be the partition and no need to think about bulk accessing of the video it will be query of individual video. 

> When designing data storage solution mention the partition. 
> 
> The best partitioning meaning the data to read from a single node and some system need consistency so the shard  by a key the represent the domain like the Tickermaster shard by the concert If to ensure the consistency.
> 
> In the example it does not need that level of partitioning so shard by videoId.

The video data store part is sample like the Dropbox, handling large blob pattern - discuss in depth. The conclusion is that the most efficient is to upload data directly to blob store like S3 via a presigned URL with multi-part upload and CDN distribution. 

The design update to upload directly to S3 meaning there will be no `POST /upload` and it will be `POST /presigned_url` the server will enable the client to upload to S3. 

The next thing will be the point that we need to take care like what details to store in a video. 

Bad solution - store the raw video. (It ignore the post-processing of data and will store what user gives. It not good different device needs video format to play video. We need to be very specific.)  

Good Solution - Store different video formats. (It involves a little bit of post-processing to ensure the video is converted to different format playable on devices. When user upload a video S3 will send a event notification to the processing service. The service will do the convert to formats and store each firmat as a file in S3. It will update the video metadata with the file URL representing the formats.)

Issue - It is not storing the small segment and the client will not able to download the part of the video.

Best Solution - Store different video formats as segments. (It involves a bit of post-processing and storing in small segment like sec length and convert the segment into different format to play in different device. It will help to process the video later.)

Issue - It will make the system complex and pipeline type as it will split the video in segment and then generate the video format per segment.  It needs to store the reference to the segments in a way that downstream to use in the streaming flow.

<img src="/images/HLDExample/YoutubeStoreVideoInSegment.png" style="width:60%;">

#### User can watch video.

The system fetch the `VideoMetadata` from the video metadata DB. The video content will be in S3 and the `GET /video` returns the metadata and it will give the URL to watch the video.


Bad Solution - Download the entire video. (It is not streaming it more like video download and playback.) 

Issue - The video to download will be a task. The video with extremely compressed and low resolution will be large to download. The main issue that user will be waiting to download the entire video like 10gb video will take 15 mins with 100Mbps.    
The client will request in HTTP and the network issue meaning the download will fail and the progress will be lost and reattempt time will be taken and in HTTP the time to donwload video will give time out.

Good solution - Download segment incrementally. (Client will download video segment to properly stream the video. The client will choose the video format like the device bandwidth and preferences like HD videos or 1080P like that and then load the first segment of the video like few seconds length. The client will load the video and continue watching.)

Issue - It is complex and relies on the uploaded videos being sorted as segment. It does not takes into account network constraints when user watching a video. In case 1080p video is streaming and network get worse then it will be slower and buffering will happen.   

Good Solution - Adaptive bitrate solution.


<img src="/images/HLDExample/VideoStreamingAdaptiveBitrate.png>







