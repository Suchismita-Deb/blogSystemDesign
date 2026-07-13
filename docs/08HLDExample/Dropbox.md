A cloud based file storage service user store and share files.

## Functional Requirement.

User should be able to upload file, download file from any device.  
User should share the file with other user and view the shared file.   
Users automatically sync files across devices.
## Non functional Requirement.



####  How you ensure file security?

We need to ensure files are secure and acessible to authorized users.

Encryption in Transit - The HTTPS protocol is used to encrypt data in its transfer between the client and the server.

Encryption at Rest - Encrypt the file when they are stored in S3. There are features in S3 to encrypt. s3 will encrypt with key and store the key separate from the file. In case anyone get the access to the file they will not able to decrypt without the key.

Access control - The `shareList` or separate table/cacheis the ACL and we should share the download link with authorized users.


What is teh solution when an authorized user share the file with the unauthorized user?

The signed URL will play the role. The user request a download link we generate the signed URL and it is valid short time. The Sign the order is then sent to the user who wants to download the file.

The sign you were in tokens anyone with a valid URL and download a file. 

The short expiration window limit doesn't fully prevent the sharing for high security scenarios we should add an additional restriction like IP bindings or signed URL with authentication cookies.