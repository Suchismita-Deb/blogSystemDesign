## My learnings and suggestion.

### Non-Functional Requirements. (3 mins).
Availability is more important than consistency. Meaning the software should be available then
a bit of mismatch in the consistency of the updates are manageble.

Low latency like <300ms the updates done in the file should be visible in near real time.

It should be scalable upto 10 Million concurrent users.

**Evaluation**.
Good point on the availability and latency and scale.

Mention like 'documents should be eventually consistent' than stating like availability over consistency. It is more clear and understandable.    

The latency of 300ms is a bit high for collaborative experience. It should be like 100 to 250ms for instant update.


No mention of the durability point like user making and update it should be there even the server crashes mid edit. User should realise that the work should be saved reliably. The system should store data in such a way that it handle server failures.

### Core Entity. (2 mins).

User
Update table - like the last update time.
Document.
**Evaluation**.
The User and Document good the update table looks like a metadata table. The entity meaning the main part of the system.

Mention the cursor and session entity and each user has the session and cursor to track where they are editing in the document. Google docs shows other user cursor in color and the selection area is an important part.

### API Design. (5 mins).
Could not able to think properly.

I mention something like - 
```
/document/user
{id of the user logged in}

/document/users
{id of the users in the same document}

/document/cursorPoint
{ It will get the users in the post and then teh cursor positions. }
```

No mention of the document creation API. The POST api to create the document with the title and return the document Id.
The API should be in a pattern GET /docs/{docId}/users and it should mention the return type of the method.

There is no real time update and persistent users and for the collaborative users the websocket is needed. A single WebSocket endpoint like WS /docs/{docId} could handle sending edits, cursor updates, and receiving changes from other users.

```java
POST /docs
{
  title: string
} -> {
  docId
}

WS /docs/{docId}
  SEND {
    type: "insert"
    ....
  }

  SEND {
    type: "updateCursor"
    position: ...
  }

  SEND { 
    type: "delete"
    ...
  }

  RECV { 
    type: "update"
    ...
  }
```

### How the user will be able to create new document? (3 mins)

My answer - 

The user will be logged in. When they create a new document, the system will generate a new document ID. If you want to share that ID with someone else for collaborative updates, another user will share that same ID with the other users, who will then make additional updates to the document at the same time.
Whenever a user makes changes, the web server will send those updates in real time to other users who are actively viewing the same page. The document will be automatically saved, and each time it is saved, the final changes will be persisted. The document will be saved to the user’s Drive in the appropriate location. By default, only the shareable link will be provided to the other users.


**Evaluation**. 4/10 

The point of document id is good but I made the discussion to the collaborative section and the creation of document is the target in this part. The request path like the client to sever to mention like he document are durably stored in the the db.  

The components are connections are not mentioned meaning the system does not explain the basic flow of creating document. A good answer should name a load balancer API layer and a db then the API get the create document request and the id generated then the write and return the id.

### How to design so that multiple user change saved concurrently in the same file? (3 mins)

My approach the document creates and the id shared with the coauthor and all will get the same partition id in the topic and get the update from teh Kafka topic partition.
