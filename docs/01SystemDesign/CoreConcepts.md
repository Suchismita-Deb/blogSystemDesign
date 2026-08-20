## API Designs.

The part is important in the API steps in the delivery framework. 

The interviewer wants to know you can design a reasonable API and then move to the complex part of the system.

There are mainly 3 API protocols - REST, GraphQL, RPC.
REST - Representation State Transfer - It uses Standard HTTP method like GET, POST, PUT and DELETE. REST maps to the db operations and HTTP semantics.

GraphQl - It uses a single endpoint and a query language that make the client specify exactly what data they need. It let the client request what it needs in a single query. Example - The mobile app that only need basic user information and Web dashboard that is the comprehensive analytics. When the discussion goes in such a way that flexible data fetching and avoid over or under fetching then GraphQL is the way.  

RPC - Remote Procedural Call - It uses binary serialization and HTTP/2 for the communication between the services. REST thinks in terms as resource and RPC thinks in terms of actions and procedures. When the service needs to validate permissions with the auth then checkPermission(userId, resource) is more natural than REST. The RPC is used for internal API in high performance and low latency.

Real time feature like chat, live updates its Websocket or SSE they are persistent connections.

### Design REST API.

**Resource Modeling**.

The first step is to understand the resource, the core entity properly.
Example - Ticketmaster the core entities are events, venues, tickets and it maps easily in REST resource.

```
GET /events                    # Get all events
GET /venues/{id}               # Get a specific venue
POST /events/{id}/bookings     # Create a new booking for an event
```

REST resource represents things and not actions. The main think should be like the entity exists in the system like events, venue and not what the user will do like book or purchase.

Resources should always be in plural nouns like events, tickets.
When dealing with eh relationship between resources there are 2 ways - nest resource, path parameter when there is clear parent-child relationship and the value is required like `events/{id}/tickets` to get all tickets belonging to the event and query parameter when resource is flat and use the query to filter and when the filter is optional `/tickets
?events_id=123&section=VIP` 

**HTTP Methods**.
GET.  
POST - create new resource. Whether you will post ticket it POST to /evenst/{id}/bookings with the booking details in the request party. The server assign an ID and return the newly created booking.  
PUT - It replaces an entire resource with what you send or create if it doesn't exist.    
PATCH - It uploads part of the resource. When an user changes the e-mail address then patch with only the e-mail address. Unlike put batch is not guaranteed to be important when a batch says _set e-mail to x_ is an idempotent but when batch says append to list it is not idempotent.    
DELETE - It is that important because repeated cost leaves the server in the same state. In case the response code differs like 204 in the initial part and next call 404.


### Passing data to APIs.

There are three main options for passing data to the rest apis. 
Path parameters - it is used when the value is required to identify the resource `events/123`
Query parameter - it is used to filter salt or modify how to retrieve results `/events?city=NYC&date=2024-01-01` This can be optional I need to return all the events without the filter. It is also useful in pagination `events?page=2&limit=20` the first option is separated by a ? and the next parameter is separated by &.   
Resource body - It contains the actual data you are sending to create or update teh resource. It is the payload.
Path parameters structure and query parameters are modified. 
Example of a booking system where the user wants to book VIP tickets for a specific events.
```java
POST /events/123/bookings?notify=true
{
  "tickets": [
    {"section": "VIP", "quantity": 2},
    {"section": "General", "quantity": 1}
  ],
  "payment_method": "credit_card"
}
```
### Returning data. 
An EPA response is made-up of two parts of the status quo and the response body.
The common status code- 200 for success, 201 for creative resources, 400 for bad request 401 for authentication required , 404for not found and 500 for server error. 
The client error 400 series and server error 500 series. 

### GraphQL.
Graphql solves a very specific problem for example the mobile app needs a very different set of data than the web app but they will stick to rest endpoints that return fixed data structure. 

In rest when two different clients need a different set of data then we have to make multiple endpoints for different use cases leading to more number of code or you have to return everything on the same endpoint leading to overfetching and slow application.  
 
Graph QL consolidated the resource in points into a single input that accepts queries describing what exactly the user want. The client specifies the shape of the response of the server returns the data in that exact format.  

In the Ticketmaster example instead of the rest endpoints we can use the graph QL endpoint and the card will look like this.
```java
query {
  event(id: "123") {
    name
    date
    venue {
      name
      address
    }
    tickets {
      section
      price
      available
    }
  }
}
```
When to use rafko in the interview - when there is diverse client with different data needs. And gives them deeper specifically asked that the mobile app needed different data than the web app. It's a good choice when the front end team need to iterate quickly without back end changes I need to add a bit of complexity. There is a need to implement query parsing, schema validation, caching strategies. 

When selecting graphql we have to think to design the schema that defines the data types and the relationship.

In the Ticketmaster example we should start by modeling the core entities as graphical types. 
```
type Event {
  id: ID!
  name: String!
  date: DateTime!
  venue: Venue!
  tickets: [Ticket!]!
}

type Venue {
  id: ID!
  name: String!
  address: String!
}

type Query {
  event(id: ID!): Event
  events(limit: Int, after: String): [Event!]!
}
```
The key difference from rest is that we defend the relationship directly in this schema. An Event has a venue and client can traverse that relationship in a single query.

The flexibility of this graphical creates N1 problem when a client requires event with their venue you have to execute one query for events and then N separate queries for each venue.

With 100 events that's exactly 101 database squared instead of 2. The solution is batching or data loader pattern that group related queries together but it adds complexity.

GraphQL also handles data authorization differently instead of securing entire endpoints like rest we secure individual field. A user might see the event name and data but not the venue data. We control this at the field level in the schema resolver.
### RPC.

RPC is a communication paradigm that allows our clients to call a procedure on a server and wait for the response without the client having to understand the underlying network details. 

Protocol like GRPC uses binary serialization and HTTP/2 making it faster than Jason over HTTP and rest API type services.  

Rest is resource oriented and RPC is action oriented. We call bump functions across a network the same way it's like the local function in the code base in the example of Ticketmaster operations using RPC will look something like.

```java
// Instead of GET /events/123
getEvent(eventId: "123")

// Instead of POST /events/123/bookings
createBooking(eventId: "123", userId: "456", tickets: [...])

// Instead of GET /events/123/tickets
getAvailableTickets(eventId: "123", section: "VIP")
```


The most popular RPC protocol today is gRPC It uses protocol buffer for serialization and HTTP 2 for transport this combo is way faster than rest in case of service to service communication.

Protocol buffer - GRPC uses protocol buffer protobuf to define service contract. We write the proto file that describes the service methods and data structure.

```java
service TicketService {
  rpc GetEvent(GetEventRequest) returns (Event);
  rpc CreateBooking(CreateBookingRequest) returns (Booking);
  rpc GetAvailableTickets(GetTicketsRequest) returns (TicketList);
}

message GetEventRequest {
  string event_id = 1;
}

message Event {
  string id = 1;
  string name = 2;
  int64 date = 3;
  Venue venue = 4;
}
```

In the single definition group client and server code in multiple languages meaning Go back end service and Java payment service can communicate with compile time type safety, catching mismatches before deployment.

When to use RPC in the interviews - In the microservice service architecture the service needs to communicate frequently and efficiently. If the interviewer mention internal service communication, high performance requirement or polyglot environment RBC is a good choice.

RPC is best when **performance is critical** (Binary serialization on HTTP2 makes it faster than Jason rest ), **Type safety matters** generator client code prevents many runtime errors, **service to service communication** (internal APIs between the services dont need REST resource semantics), **Streaming is needed** (gRPC supports bidirectional streaming for real-time features).


In a system for the public and in points like the mobile apps and the web clients we can use rest API and Group C for the Internet communication like booking services payment services or inventory services. Unless asked no need to go in this direction let's just stick to REST.
### API Patterns.

The API patterns applies to REST, GraphQL, RPC.

**Pagination**
When dealing with large amount of data set we cannot return everything at once example returning events ever created would return in gigabytes of data. We need pagination to break each result into manageable chunks. 
There are two main approaches of **pagination- offset based** and **cursor based**. 
Offset based pagination - we specify how many records to skip and how we need to return `/events?offset=20&limit=10` gets records 21-30 it's easy but it has a problem with large data set. If someone adds a new event while you're paginating through records we might see duplicates or missing records.

Cursor Based Pagination - It solves the issue by using a pointer to a specific record instead of counting from the beginning. The first request `/events?limit=10` its response includes the event plus the cursor pointing to the last record `{
  "events": [...], "next_cursor": "cmd9atj3p000007ky19w1dpy2"
}` the next request would be `/events?cursor=cmd9atj3p000007ky19w1dpy2&limit=10`
The cursor is typically encoded reference of iid or timestamp it is stable because it is not affected by new records being added. It could be difficult to implement the feature like jump to page #5 because in the example the next cursor ID is the ID of the last event in the first page. 
 
The offset based pagination is fine unless you're dealing with real time data and interviewer is asking for high volume scenarios.

**Versioning strategy**

API evolves and we need a strategy to handle change. It's important for public APIs where we cannot control when clients update their code.  
 
The most common is URL versioning where include the version number in the path `v1/events` It's easy and client will know exactly what version they're using.  
 
Header versioning it puts the version in an header instead `Accept-Version: v2` or `API-Version: 2` 

The most common thing is to go with this URL versioning. 

### Security.
The security part of the discussions will give some added benefits a basic correct principles signals gives a good point.
### Authentication and Authorization.
Authentication- it verifies the identity providing the user name and password or token. It is the process of verifying who you are.
Authorization - it verifies the access level and permission in case teh user is allowed to perform the specific action requested.

There are many two ways to handle this thing API keys versus JWT tokens. It is not the main thing with the design we can call out like which endpoint needs the user to authenticate and say that we'll be relying on the JWT to store the user session in the database to authenticate the user.  

**API Keys**.

API keys are long randomly generated string that adds like the password for the application rather than the humans while a client makes the request they include the API key in the authorization header on the server look up to that key to understand which application is making the request. 
The first step is to generate the API key for the client and store it into the database along with any permission or rate limit for that client and then verify each incoming request by looking up the key.  
They are perfect for server to self communications where you can control both sides when we're making a call like payment service then an API key is effective. It is also find use API key for any third party app applications who need programmatic access to your system. 
 
When making an user facing product like user facing APIs then the API key is not the right option. Users shouldn't be managing this long strings I need my keys do not expire or carry user context the way user session needs to.

Example - `GET /events Authorization: Bearer sk_live_abc123...`

**JWT Tokens**.

It includes the information directly into the token itself rather than storing the session state on the server. 

When an user logs in then the server creates the JWT containing the user ID permission and the expiration time then sign the entire token with the secret key. When the GWD comes back with future requests we can verify its authentic by checking the signature and you can read the user information directly from the token with alternate database lookup.  
 
It works well in the distributor system because any service with access to the key can validate tokens independently. When a mobile app sends AGWT to the API gateway the gateway can verify the users identity and forward the request to the booking service.

```java
// JWT payload
{
        "user_id": "123",
        "email": "john@example.com",
        "role": "customer",
        "exp": 1640995200
        }
```

API key for internal service communication and external developer access and JW tokens for user sessions in web and mobile application.  
 
JWT token can be stateless no database lookup required and can carry user context making them ideal for user facing applications. 

**RBAC Role Based Access Control** 
Real system have different types of users with different types of permissions. When the Ticketmaster example customer can book tickets and view their bookings if any manager can create events and view the sales report. 
 
RBAC assigns rule to users and permissions to roles 

```
Roles:
- customer: can book tickets, view own bookings
- venue_manager: can create events, view sales for their venues
- admin: can access everything

User: john@example.com → Role: customer
User: manager@venue.com → Role: venue_manager```
```

### Rate limiting and Throttling.
Per user limit - 1000 requests per hour per authenticated user. 
Per IP limit - 100 requests per hour for unauthenticated users. 
Endpoint specific limits - In booking attempts per minute to prevent tickets scalping speed. 

The rate limiting is implemented at the API gateway level and when the limits are exceeded in return 429 too many requests status code.


## Data Modeling.

Data modeling is the process of defining how the application's data is structured, stored and related. The it specifies what entity exits and how they are connected.   

In the data engineering interview there are specific rounds for data modeling but the system is an interview the difficulty is very less.

The target is to make a complete schema design the clear and functional and aligned with your systems requirement.

The data modeling comes two places in the interview one at the time of **core entity** where we will be mapping one to one with the table and the collections to from the backbone of the schema and in the **high level design** step where we will be sketching the basic schema along with the database components and include the key fields, relationship and note where to use partition to support the main query patterns.

The board should look like this in the diagram.

| **Posts** | **Comments** | **Users** |
|-----------|--------------|-----------|
| postId (pk)<br>userId (fk, index)<br>content<br>mediaUrls<br>createdAt (index) | commentId (pk)<br>postId (fk, index)<br>userId (fk, index)<br>content<br>createdAt (index) | userId (pk)<br>name<br>email (unique)<br>createdAt |

The first target - Type of db. Most of teh time PostgreSQL.

Relational databases organizes the table into fixed schemas where the role represents the entities and the column represents the attributes. The enforced relationship through foreign queue and provide acid guarantees for transaction. 

The system designs main function directly maps onto one of these models for example ecommerce application the user product order payments etcetera are directly placed into the tables with the constraint and foreign keys.

Seeker can easily manage complex queries for example get all the posts by the user that a given user follows order by recency then it's pretty straightforward by joining the table. 



Mentioning a lot of complex reporting query styles raises a yellow flag about the performance only mention when it is needed also mentioned in case the denormalized views or cache or precomputing result helps.

There is a misconception of the scalability but modern secure database can scale with techniques like read replicas, sharding, connection pooling, caching and large companies like Facebook and Airbnb rely on relationship databases. Scaling often matters in the way you architect the design.

### Document Database.

### Key-Value Store.

### Wide - Column Database.

### Graph Database.

[]
### Conclusion.

The data modeling part of the design should only focus on making the system requirements fulfilled start by outlining the core entities at the early in the interview and then introduce the database components during the high level design.  

Determine the type of the tabbies we're gonna use- give the names of the column that is needed to fulfill the functional requirement for each entity - Mention the primary and the foreign key for each relationship - determine which column needs indexes - Determine in case denormalization needed for the performance - verify if charging is needed if yes then get the Shard key that matches the access pattern.