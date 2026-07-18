A cloud based file storage service user store and share files.

## Functional Requirement.

User should be able to upload file, download file from any device.  
User should share the file with other user and view the shared file.   
Users automatically sync files across devices.
## Non functional Requirement.


## High Level Design
### User should be able to upload file from any device.

The files storage imp part like the file content and the metadata of the file.

Metadata we can use NoSQL like DynamoDb. Dynamo DB is a fully managed nosql DB hosted by AWS you need the metadata is loosely structured with few relation and main query pattern being to fetch files by users it makes dynamodb a solid choice. 


The schema will be a simple document.
```sql
  {
    "id": "123",
    "name": "file.txt",
    "size": 1000,
    "mimeType": "text/plain",
    "uploadedBy": "user1"
  }
```
There are some solutions to tackle this issue starting from naive to advanced - upload files to a single server - store file in BLOB storage - upload file directly to BLOB storage.

The naive solution - Upload file in single server.
The simple solution is to directly approve the file to the back end server something like file service type of services that will store to them . The POST /files Will accept the file and then store it into the server's local file system I'm saving the metadata into the database  

It's applicable for small applications but it won't scale and it will not be reliable.
The fileMetadata will contain details like fileId, name, size, uploadedBy.

Issues- there are multiple issues . The number grows we need to add more storage to the server horizontally scale or by adding more servers . 

It is not reliable . In case the server goes down we will lose all the data in the file and it need more reliable solution to handle failures and scale with easy.

The better solution is to store the file in the BLOB storage.

A better solution is to use the BLOB storage like Amazon S3 or Google cloud storage when the user uploads a file to the back end we send the file into the BLOB storage to store and store the maintenance into the database . The scaling part as resolved as BLOB storage can handle the scale for us and it's more reliable if the server goes up data would be lost next thing next thing we will also use the BLOB storage features like lifecycle policies to automatically delete the old files and version you need to keep to the track of the new files.

Issues - the entire design will be more complex because we need to integrate the BLOB storage with the service and handle the cases where the file is uploaded and metadata is not uploaded.

We will solve this issue using some transactional approaches where the file would only save the metadata if it is successfully uploaded. 

The approach requires the file to be uploaded 2 times one to the back end service and another one to the BLOB storage this is redundant and then we can solve these issues by allowing storage 


The best solution is upload file directly to the BLOB storage.

The best approach is to allow the users to deliver the BLOB storage from the client it's faster and cheaper than uploading to the backend.

 We should use the pre signed URLs to generate the URL that the user can use to upload the file directly to the BLOB storage. When the file is uploaded the BLOB storage will send a notification to the back end to save the metadata. 

Pre signed URLs are URLs that give the user permission to upload the file to the specific location in the BLOB storage service. We will send this URL to the user when they want to upload the file  

The initial API call was simply a post call but now there are three steps.

Step number one request a pre signed URL from back end generate the URL using these three SDK and save the File metadata to the user with the status of uploading.

```http request
POST /files/presigned-url -> PresignedUrl
Request:
{
  FileMetadata
}
```
User will use the percent you find the BLOB storage directly from the client's end and This is why I put request directly to the pre signed URL at the file is the body of the request.
When the file is uploaded the BLOB storage service will send a notification to the back end using a S3 notification and the back end will app upload the metadata into the database.

UploadFileFromAnyDevice.png
#### User should be able to download a file from any device.

There are three  possible ways starting from nave to advanced - Download from file server, download from BLOB storage, download from CDN  

download from CDN.

The best approach is to use a content delivery network CDN to cache the file closer to the user.
A CDN is a network of servers distributed across the globe that cache files and serve them to user from the server closest to them.
Introduces latency speed of the download times . For security it is similar like the history P signed URL but will generate a URL that the user can use to download it from the CDN. This URL will give the user permission to download the file from the CDN for a limited time.

![DownloadFileFromAnyDevice.png](..%2Fimages%2F08HLDExample%2FDownloadFileFromAnyDevice.png)

Issues - CDN are relatively expensive.
It is common to be strategic about what file had to be cached in for how long period.  
We can use the cache control header to specify how long the file will be cached in the CDN.  
We can use a cache invalidation mechanism to remove the files which are not getting used or which are being updated or deleted.   
This way only the recent file access starting the CDN. 

### TODO.

### Common Deep Dives.
#### How do we support large files? 
The first point is the user experience. The two insights - Progress indicator and Resumable uploads.

Progress Indicator - Users should be able to see the progress of their uploads so that they know it's working.
Resumable uploads - Users should be able to pause and resume the uploads and in case they lost Internet connection or close the browser they should be able to pick up where they left say 50 % of data already uploaded.

There are many limitations that will come when uploading a large file via a single post request.
**Timeouts** - web servers and clients have a timeout settings to prevent this indefinite waiting for the response - A post request for a 50 GB of file would easily exceed these timeouts. 

Example A 50 GB of file and Internet connection of 100 megabits per second the time it will take to upload the file - `50GB * 8 bits/byte / 100Mbps = 4k secs` It means `4000/60 = 1.11 hours`. 

Browser and servers limitation - in most of the cases the upload of 50 gigabytes of file is not even possible with post request due to the browser or the server's limitation as they impose a limit on the size of the request payload . 
When Apache and nginx can be configured to accept large payloads most modern services like API gateways have hard limit that are much lower and cannot be increased. It's mostly like 10 megabytes in case of Amazon API gateway.
Network interruptions larger files are supposed to face the network interruptions and in that case uploading and the entire file needs to be restarted from the scratch  

 
User experience- users are effectively going to have no clue on the progress of the upload as there is no idea how long it has been done or even it's working.

To address this issue we will be going with chunking to break the file into smaller pieces and upload them one at a time or in parallel . Junking needs to be done on the client side so the file can be broken into smaller pieces before it sends to the server.   

There is no point of putting chunking into the server side because at the end of the day the user is entirely uploading the file into the server which beats the purpose  
When you talk about chunking we typically broke the father into this five to end and MB pieces and then address based on the network conditions and the size of the file. It's very straightforward to show the progress indicated to the user and we simply track the progress of each chunk and update the progress bar to the user.

The follow up part is how we will handle the resumable uploads? 
We need to track which junk has been uploaded and which have not and we can do that by saving the state of the upload in the database especially in the `FileMetadata` table.
The sample code of the FileMetaData table update the chunk field.
```sql
{
  "id": "123",
  "name": "file.txt",
  "size": 1000,
  "mimeType": "text/plain",
  "uploadedBy": "user1",
  "status": "uploading",
  "chunks": [
    {
      "id": "chunk1",
      "status": "uploaded"
    },
    {
      "id": "chunk2",
      "status": "uploading"
    },
    {
      "id": "chunk3",
      "status": "not-uploaded"
    }
  ]
}
```
When the user resumes the upload we need to see the junk field is uploaded or not uploaded . We will continue with the floating part from the place it is not uploaded yet and that way user don't have to start the upload from the scratch  

How do we ensure that the junk field is kept In Sync with the actual junk that has been uploaded ?

There are two ways - Update on client Patch request and Server-side chunk verification.

**Update on client patch request.**

The most easy approach is to use the client to orchestrate the chunk status . The flow would look like the client takes the file, chunk it and upload to S3. S3 respond to each chunk uploads with a success message . Upon success the client send a patch request to the back end to update the junk field in the field mirror in the table.
```http request
PATCH /files/{fileId}/chunks
Request:
{
  "chunks": [
    {
      "id": "chunk1",
      "status": "uploaded"
    },
  ]
}
```

Issues - you're asking the client to keep the chunks filled In Sync with the actual chunk has been uploaded . A malicious user can send a patch request to the backends to make all the junk as uploaded without actually uploading them. In that case they would anyway corrupt their own uploaded files and not anyone else but still it's a risky to keep the data inconsistent which will be difficult to debug. 

We can address this issue by using a server side approach to ensure that the chunks field stays In Sync with the actual uploaded chunks.

Server side junk verification  

The best approach is to use a server side verification using ETags.

### TODO.

#### How we can make the upload download and sync fast ? 
To download we use the CDN to cache the file closer to the user. It will not let the file to travel far to get it to the user, reducing latency and speeding up the download times.  
 
For upload the chunking will be helpful and in  resumable uploads also play an important role in speeding up the upload process.  
When the bandwidth is fixed (the upload pipe is only so big) we use chunking to make most of the bandwidth. 
We can send multiple chunk in parallel and utilize adaptive chunk size based on network conditions we can maximize the use of available bandwidth. 

The same chunking is going to be useful for syncing file when a file change we only need to sync the chunk that actually changed rather than the entire file making the sync faster  
 
Fixed size junk like 5 MB meaning inserting a single byte near the beginning of the file will shift all subsequent chunk boundaries and it will cause every chunk after the edit they produce a different fingerprint it. It will make the delta sync useless.
The solution is Content Defined chunking CDC where the chunk boundaries are determined by the file's content using the rolling hash like Rabin fingerprint. With CDC a small edit will only affect the chunk immediately surrounding the changes and the vast majority of the chunk remains identical. Dropbox uses it in practice  

The enhanced way we can use compression to speed up the upload and download. Compression reduces the file size meaning fewer bytes to transfer  
We're uploading directly to S3 the compression happens in tenure on the client side and the compressor is stored in S3  
 
When downloading the client decompresses the file after retrieving it. It will keep the back end out of the data path and still benefit the reduced file transfer.
We need to be careful on when to compress because compression is only useful if the speed gained from transferring fewer bytes give more benefit than the time it takes to compress and decompress the file.

Files format like images and videos the compression ratio is very low and it doesn't worth to compress and decompress the files in text file the compression ratio is much higher and depending on the network condition it is actually worth it A5 GB text file can be converted to 1GB or even less depending on the content.

There should be logic on the client side that will decide whether or not to compress the file uploading based on the file size type and network condition.

There are many compression algorithms to compress files like gzip Zstandard. Each of the algorithm has its own tradeoff in terms of compression ratios and speed.

Gzip - Most widely used. Broad support everywhere.

Brotli - Better compression ratios than Gzip (esp. text). Supported by all modern browsers (Chrome, Firefox, Edge, Safari)

Zstandard (zstd) - Excellent balance of speed & ratio. Much faster compression/decompression than Gzip. Tunable across wide speed/ratio tradeoffs. Strong choice for client-side compression (e.g., Dropbox)

The file should always compress before it's encrypt in the case where encryption is necessary. Encryption introduces randomness into the file and it will make it difficult to compress.
####  How you ensure file security?

We need to ensure files are secure and acessible to authorized users.

Encryption in Transit - The HTTPS protocol is used to encrypt data in its transfer between the client and the server.

Encryption at Rest - Encrypt the file when they are stored in S3. There are features in S3 to encrypt. s3 will encrypt with key and store the key separate from the file. In case anyone get the access to the file they will not able to decrypt without the key.

Access control - The `shareList` or separate table/cacheis the ACL and we should share the download link with authorized users.


What is teh solution when an authorized user share the file with the unauthorized user?

The signed URL will play the role. The user request a download link we generate the signed URL and it is valid short time. The Sign the order is then sent to the user who wants to download the file.

The sign you were in tokens anyone with a valid URL and download a file. 

The short expiration window limit doesn't fully prevent the sharing for high security scenarios we should add an additional restriction like IP bindings or signed URL with authentication cookies.

It also worked with modern students like cloudfront and it is a feature of S3.    
Generation - A signed URL is generated on the server including the signature that is add it to the URL path, an expiration timestamp and another restrictions like IP address. In cloudfront this signature is created using the content providers private key.

Distribution the signed URL is distributed to an authorized user who can access the specified resource directly from the CDN  
 
Validation - When CDN receives the request with a signed URL it verifies the signature using the public key (registered in the CloudFront) and see the expiration timestamp and other restrictions. When the signature is valid the URL has not expired then the CDN serves the request content in case not then it denies access.