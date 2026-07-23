### Functional Requirement

User create a profile with preference like age, range, interest and maximum distance.

User will see profile based on preferences.  
User swipe left or right on profile to express yes or no.  
User can see the match if both user swipe right on each other.
Out of scope.  
Users should be able to upload pictures and chat via DM after matching  
User can send super swipes and purchase premium features  

The design is mainly focusing on the user recommendation feed and swiping feature and not some other auxiliary features Get this thing clear from the interview to understand what to mainly focus.
### Non functional Requirement

Does it seem to have a strong consistency for swiping if an user is swipe yes then that user who already have swiped yes should immediately get the match notification.  
System should scale to lots of daily users or concurrent users likely 20 million daily active and across 100 swipes/user/day on average.  
The system should load the potential matches staff with low latency like 300 ms.  
The system should avoid showing user profiles that the user has previously swiped on.

Out of scope.
The system should protect against fake profiles and the system should have monitoring and alerting.

### Planning.

