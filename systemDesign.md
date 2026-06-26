# System Design Interview Guide 

I used https://interviewing.io/guides/system-design-interview. 

1. Don't name drop a specific technology unless you really know it. Instead, say something like a queue or a NoSQL database. 

## Part 2: 15 Fundamental System Design concepts 
1. It is more important to cover everything more broadly, than everything in small detail. The goal is to get an MVP by the end of the interview. 
2. Whenever you make a decision, talk about the why. So this is the tradeoffs of your decision. 
3. Keep it simple. Don't include something complicated like a distributed system unless you need to. 

### What to Say If You Don't Know 
1. If you are sort of familiar with the idea: 
    - I don't know, and I'm definitely going to look that up right after this interview, but if I have to give my best guess I would say [X], and here is why. 
    - I don't have hands on experience with this, but I have read about it, and here is what I know / and so I would approach it like. 
2. If you've never heard of what they are talking about, then just tell them you have worked on XXX and have never heard of that. 


#### To Skip Handwave stuff
1. I'm going to skip going into [detailed thing] for now, but if we want, we can come back to it later. 


#### Cold Interviewer: 
1. Correct me if I'm wrong 
2. Stop me if I'm going off track, but I think the next thing to do is X because of Y.


1. Check in at major milestones, so when you're taking requirements, or when you're done with the high level design. 


### 12 Fundamental System Design concepts 

#### API (Application Programming Interface)
1. An API is a set of rules or contracts through which two or more software entities communicate. For each of these, the question is "How does the client tell the backend server what it wants?"
2. There are three API architectural styles.
    1. REST (Representational State Transfer): REST organizes an API around resources. This means nouns such as: users, orders, products, which each get their own URL.
        - REST is like choose the noun, and then choose what to do with it (GET, POST, PUT, PATCH, and DELETE). 
        - This should be used when you have a standard API involving the users, orders, products, documents, or other resources.  
        - Strengths:
            - This gives us a structured way of getting and modifying information from our database. 
                - But the sever controls what you get back for each response. So the user might get information it might not need, and so this is called over fetching. 
                - Or the frontend might have to make several requests to collect related information. This would be called under fetching. 
            - It is the most universally used and works for most circumstances. 
        - Weaknesses: 
            - You have to write the requests for each type of entity in your database, in comparison for GraphQL, where you don't have to make separate inquiries to grab all the data the called needs. 
            - Not as space efficient as RPC. 
    2. RPC (Remote Procedure Call): Think of this as calling a function on another computer. 
        - This should be used when your system primarily exposes actions, computations, or internal service operations. It is useful for running commands such as running models, processing files, and coordinating microservices. 
        - Strengths:
            - This works well when the system has clear operations that do not map neatly to the basic create, read, update, and delete actions. 
            - It is common for communications between internal services. It uses compact binary messages, and so it can be smaller and faster than the JSON based REST APIs. 
        - Weaknesses: 
            - A remote function call is not the same as a local one. 
                - A local function call either runs or it errors out. A remote call can have network delays, connection failures, and timeouts. 
    3. GraphQL (Query Language): Tell the server exactly what information you want returned through a query. 
        - Use this when the frontend needs flexible access to interconnected data and frequently needs different combinations of fields. 
        - Strengths: 
            - This is best for frontend heavy applications so that the frontend can request the exact data that is needed for each page. 
            - This is useful when different screens require different data. This keeps the backend from not having to create a separate endpoint for every screen. A desktop page may request: 
                ```sql
                customer {
                    name
                    email
                    address
                    orders {
                        id
                        total
                        items {
                        name
                        price
                        }
                    }
                }
                ```
            And a small mobile screen may request only: 
            ```sql 
            customer {
                name
            }
            ```
        - Weaknesses: 
            - This moves significant complexity into the GraphQL server. 

#### Databases (SQL vs NoSQL)
1. SQL (Structured Query Language): This is a relational database that is composed of tables where each row reflects a data entity and each column defines specific information about the field. 
    - This can be thought of as a collection of organized spreadsheets that are connected to one another. So instead of storing all information in one giant table, you separate it into related tables. 
    - Normalization: We update the address in one place, and other tables might reference it. So we avoid duplicated data.
    - Strengths: 
        - ? 
        - Strong ACID guarentees.
            - One of the most important SQL concepts is a transaction. A transaction is a group of database operations that should behave as one unit. 
                - Say someone transfers $100 from Account A to Account B. Both of the operations need to happen, and so SQL provides strong transaction support through ACID. 
            - Has strong consistency. This means that the service will always see up to date information, even if there is a slight delay.
            - ACID: Atomicity, Consistency, Isolation, Durability
                - Atomicity: This means that everything is changed without failures, or everything is reverted. This keeps us from having half the writes changed, which can lead to inconsistencies. 
                    - Either the entire transaction succeeds or none of it does. Think of it as all or nothing. 
                - Consistency: We are guarenteed to be internally consistent. This hapens since certain parts of the database are locked when it is being written, which forces other requests to wait. This means that when you query the database, the data might be stale for a couple of seconds before it becomes consistent. 
                    - This means that a transaction must leave the database in a valid state. 
                    - Think of this as: an order has to belong to a user, two users cannot have the same unique email address. 
                    - This meakes sure that the database rules stay true before and after a transaction. 
                - Isolation: Transactions should not interfere with each other in unsafe ways. 
                    - Say that both people try to buy the last concert ticket at the same time. 
                    - Isolation will make the database behave such that the transactions happened in a controlled order. 
                - Durability: Once a database has confirmed a transaction, the change should survive the database crashing or restarting. 
    - Weaknesses: 
        - SQL uses B-Trees, which means that they are slower to write to.
        - SQL can handle schema changes, but changing schemas will be more explicit and controlled.  
        - It does not work well for mixed schema data
2. NoSQL (Not Only SQL): This is a nested key-value store. This is pretty much a catch-all term for many other technologies which are not relational.
    - In general, it is faster for writes, but it is slower to query. 
    - It is common for denormalization, where we duplicate data to make reads simpler/faster. 
    - These systems are designed around partitioning (also card sharding). 
        - So an example would be users are divided across servers. This means that each server handles only part of the dataset. 
    - The common aspect of all these databases is that they do not require rigid schemas like SQL models. There are four types of NoSQL databases:
        - Key Value: Stores data with unique keys. 
            - Good for caching, user sessions. 
            - They are great when you know the key, but they are less natural when you need complex queries like "Find every user in DC who placed morre than five orders last month".
            - Examples: Redis
        - Document: similar to key value, but they have the ability to perform aggregate searches across data and store it in a variety of formats like JSON, XML, and YAML. This is the most common. 
            - Examples: MongoDB 
        - Columnar: These are designed for very large datasets, and for queries based on known keys and ranges. It is for data distributed across many machines. 
        - Graph: Store complicated node and edge relationships. They also have code for graph traversal and modification. 
3. To decide whether you need SQL or NoSQL, ask yourself the following questions: 
    - Is it important for your data to have structured relationships?
    - Do you need strong consistency? With strong ACID guarentees?

#### Scaling
1. Scaling means increasing a systems capacity so that it can handle more users, requests, data, or computation without becoming slow or failing. 
2. There are two ways to scale: vertically and horizontally. These are not opposing ideas, most architectures use a combination of both scaling types. 
3. Vertical Scaling: upgrade your computers hard drive, CPU (Central Processing Unit), and RAM (Random Access Memory). 
    - RAM temporarily stores short term memory. 
    - This is the easiest way to scale initially since you don't have to change your architecture. 
    - It also has less latency since the communication is done locally on one computer, instead of through a network. 
    - Disadvantages: 
        - At one point, it is either too expensive, or impossible to upgrade a computers hardware past a point. 
        - This can create a single point of failure. 
        - Upgrading may require downtime. 
4. Horizontal Scaling: buy more computers. 
    - This relies on building systems that communicate between multiple computers to store and process information. A load balancer usually decides which server recieves each request.  
    - There is database scaling and compute scaling. 
    - Database scaling: this is used when one database cannot store or serve all of the systems data. 
        - There are two common approaches for this: replicating and sharding. 
        - Data Replication: Keep copies of the same database on multiple machines. 
            - The primary database handles writes, and the replicas handle reads. 
            - This helps because most of the time a system will have way more reads than writes. 
        - Sharding: Dividing one large database into multiple smaller databases. 
            - Each shard contains only part of the data. And a routing rule determines which shard stores a specific item. 
    - Compute scaling: this is needed when one machine can't process all the work quickly enough. 
        - This lets you split a job up into multiple, and let those jobs run in parallel. 
5. How horizontal and vertical scaling are used together: 
6. Caching does not replace horizontal or vertical scaling, but it is another technique that reduces the pressure on the rest of the system.

##### Requests per Second 
    - Try and convert users to requests whenever possible. Assume that: 
        - There are 20 requests per active user per day.
        - Peak traffix is 5x the average traffic. 

| Peak RPS | Practical Rule |
| --- | --- |
| Under 100 | Low: One application server |
| 100 - 1,000 RPS | Moderate: Use a few stateless servers behind a load balancer |
| 1,000 - 10,000 | High: Horizontally scale application servers; add caching |
| Over 10,000| Very high: Distributed infrastructure, caching, queues, replicas, and possibly sharding |

##### Decision Table
| Workload | Default Architecture |
| --- | --- |
| Under 100 RPS, under 100 GB| One server and one database |
| 100 - 1,000 RPS, under 1 TB | Load balancer plus a few app servers, and one primary database |
| 1,000 - 10,000 RPS | Horizontally scaled app servers, cache, and database replicas |
| Over 10,000 RPS| Distributed app tier, cache, queues, replicas |
| Over 10,000 writes/sec or over 10 TB | Shard |
| Over 10,0000 RPS| Assume internet scale distributed design |

#### CAP Theorem
CAP is for Consistency, Availability, and Partition Tolerance. The CAP Theorem states that a distributed system (when data is stored across multiple machines) cannot achieve all three, so systems should be designed to achieve two out of these three. 

1. In most systems, we need fault tolerance, so the choice is between consistency and availability. 
2. You need to ask: what is worse, incorrect data, or downtime?
3. Consistency means that every node in a network will have access to the same data. 
    - So every read will return the most recent successful write, regardless of which server handles their request. 
4. Availability means that the system is always available to users. 
    - So every request sent to a working node receives a response. 
5. Partition tolerance means that the system continues operating when nodes cannot communicate with one another. 
    - This can happen due to network outages, regional cloud outages, etc. 
    - In a distribute system, network failures are unavoidable. So all of our systems need partition tolerance. 
6. CA systems: this means there is consistency and availability when there is no partition. 
    - A single database can behave like a CA system. 

#### Web Authentication and Basic Security
1. Authentication: verifying the identity of our service's users. 
    - Storing Passwords: 
        - Don't store the username and password by itself. If there is a leak, this would compromise users, and people are known to use the same password for many places. 
        - A hash function takes in an input and maps it to a smaller value. 
        - A crypotographic hash functions are very hard to crack. 
            - General purpose hashes like SHA-256 are fast, and so we shouldn't use them since then attackers can test billions of guesses quickly. 
            - Use something like bcrpyt, or scrypt. These increase the computational cost of each password guess. 
        - Use the hash table to store the password's hash. 
    - Salting: Instead of hashing the password, attach a random word to the password and then hash it. 
        - This is used to render rainbow tables unusable. 
    - Real World Password Hashing: 
2. Web Sign In: 
    - Session Token: 
        - This is basically equivalent to a password. So don't store it in your database in plaintext. Also salt and hash it. 
        - This is sent with every token. 
        - These should have a short expiration date. This does not mean the user has to log in again, you can rotate them silently in the background. 
            - This limits the amount of time the user can be stay logged in without visiting your site. 
        - Encrypt all traffic with HTTPs. 
            - HTTPS encrypts all the network traffic between the client and server, instead of just an endpoint
    - JSON Web Tokens (JWT): BBLAH BLAH BLAH!!!!!! 
3. Cookies: This is used to store the session token or JWT on the client side. 
    - BBLAH BLAH BLAH PT2!!!!!! 

#### Load Balancers 
A load balancer sits between the client and backend servers. It distributes the incoming requests across those servers. 
1. Round robin: if there are n machines, then the load balancer will send requests to each of them one by one.
    - A limitation of this, is that it does not consider the current load on each server. 
        - So Server 1 could still be processing some expensive requests, while Server 2 is idle. 
2. Least connections / response time: send request to the most free machine. 


#### Caching 
Caching is storing an expensive computation so that you don't have to compute it again. 
1. We use caching to reduce an expensive network computation/number of network calls/database queries. 
    - This means it is more expensive with storage. This is typically stored in RAM. 
    - Caching is used for read-heavy systems like Twitter and Youtube. 
    - Use the 80/20 rule. This states that you want to store 80% of read requests in 20% of storage/memory. 
        - We can have caches store data in local regions. 
            - A popular video in the USA would probably not be a popular video in Korea. 
2. The two popular caching patterns are casche aside pattern and write through pattern and write back. 
    - Cache-aside pattern: Here, an application will try to fetch data from cache. If the data is not found (this is called a cache miss), it will fetch data from the database or do an expensive computation, and then put that data back to the cache before returning the query back to the user. 
        - Stregths: 
            - We only cache the data that we need. 
        - Weaknesses: 
            - The data can become stale if there are lots of updates to the database. 
            - If there are a lot of cache misses, then the application has to do a lot more work. Since now it is hitting the cache, and then going to the database, instead of just going to the database. 
    - Write-through and Write-back patterns:
        - Write-through: This is when we synchronously write data to the database.
        - Write-back: This is when we asynchronously write data to the database. 
            - We put the data in a queue, which writes the data back to the data. This will help with improving the latency of writes on your application. 
        - A disadvantage of this, is since we are writing all the data to the cache, some of the data might not even be read. This means we are overloading the cache. 
3. Using caching on the client side: 
    - Cache movie recommendations for a user in their device or browser. 
4. Cache invalidation: 
    - We don't want to keep stale stuff in the cache. 
    - So we need a way to expire or invalidate data from the cache. 
    - This can be done with LRU (Least Recently Used)

#### Message Queues: 
1. A message queue is a temporary holding area for tasks or data that need to be processed. 
A async system is one where the client sends a request, and does not wait for the message to be delivered. 
2. The client does not expect a response. This function/API is called a "fire and forget" call. 
    - Can think of this as when someone places an order, they aren't immediately waiting for a confirmation email. 
3. The clients message is not immediately sent over to the processing system, but is instead sent to the broker/message queue. 
4. Advantages: 
    - Since a queue stores messages that need to be stored in a database, we can put them here. There is a possibility that a lot of traffic can cause the CPU of the database to go up, which can kill the server. 
        - This would cause the database to go down and probably loose the data. 
    - If a message has to be processed by exppensive code, you can put them in a queue while the previous messages are being processed so that you don't overload and/or kill the servers. 
    - A queue can deliver messages to multiple systems, instead of having the client do that. 
    - The client does not need to know the server address now. 
5. **Idempocy???**


#### Indexing
1. A database index is like the index at the back of the book. 
2. To find a record without an index, we would have to do the following. Disk access is a lot slower than searching data that is already in memory. 
    - Load a block from disk into RAM 
    - Search the records inside the block 
    - Load another block if the record was not found
3. There are two types of indexes: dense and sparse. 
    - Dense: A dense index contains an index entry for every records. 
        - Strength: very quick and easy look up 
        - Weakness: 
            - Requires more storage 
            - Must be updated whenever records change
    - Sparse: A sparse index contains entries for only some records, usually one entry per block or group of records.
        - Strength: Less storage 
        - Weaknesses: 
            - Now you have to search within the selected block 
            - Usually means the underlying data has to be sorted
4. Multilevel indexes: This is used when the database grows. 
    - Creates an index for the index. 
5. B-Trees: 
6. B+ Trees: 


#### Failover 

#### Replication 

#### Consistent Hashing 

### Part 3: Three Step Framework 

The three steps are: 
1. Requirements 
2. Data types, access patterns, and scale 
3. Design 

#### Overview of the 3 Steps 

1. Step 1: Requirements 
    - Inputs: Problem statement from the interviewer. 
    - Outputs: List of functional and non functional requirements
2. Step 2: Data Types, API, and Scale 
    - Inputs: 
        - Functional and non-functional requirements. 
        - Problem statement from the interviewer. 
    - Outputs: 
        - List of data types we have to store 
        - Access patterns for these data types 
        - Scale of the data and the scale of the requests the system needs to serve. 
3. Step 3: Design
    - Inputs: 
        - Functional and non-functional requirements. 
        - Problem statement given by your interviewer 
        - List of Data Types we need to store 
        - Access patterns for these data types (API)
        - Scale of the data and the requests the system needs to serve 
    - Outputs: 
        - Data storage 
        - Microservices 

#### Step 1: Requirements
- Inputs: Problem statement from the interviewer. 
- Outputs: List of functional and non functional requirements

1. Even if you are familiar with what they are asking you to design:    
    ```
    I'm not familiar with the platform. Would you be able to give me a high-level overview of what we are looking for?
    ```
2. Functional Requirements: the core product features and use cases that the system has to support. 
    - The goal of the questions here is to figure out what needs to be built. Nothing about how or the scale. 
    - Ask just enough questions to gather all use cases for the system. 
    - Identify the main business objects and their relations. 
        -  For Twitter it would be accounrs and tweets. Then think of the relationship between those two. 
    - Then go deeper into what makes up each object of interest. So what makes up a tweet? Can it have media?
    - Think about if the objects are mutable. 
    - Access Patterns, split this into reads and writes. 
        - Given object X, return all related objects Y. 
3. Non-Functional Requirements: These are quality attributes that specify how the system should perform a function. 
    - This is centered around: performance, availability, and security. 
    - Performance: How quickly the system responds to user requests. 
        - We would want to optimize for performance when we have syncronouse user facing workflows. 
            - This means that the user is expecting an immediate response. 
        - Also for workflows that are accessed most frequently. 
    - Availability: how much downtime can the service have 
        - We can take a hit on availability for consistency. 
Design Tiktok: 
- Objects: Users and Posts 

- Users: 
    - Can follow other users 
    - Can like posts 
    - Can delete their accounts 
    - Contains information such as username, password, and posts created by them
    - Can comment on or repost posts 
- Posts:
    - Contains words, and media (video, audio)
    - Has a like count, comments 


- Ask if the videos are mutable. 


Access Patterns, split this into reads and writes. 
Given object X, return all related objects Y. 
1. Given a user, get all videos they have posted. 
2. Given a user, get their feed (so this would be videos posted by people they follow)
3. Given a video, get the likes and comments. 

Writes: 
1. Post a video 
2. Follow an account 
3. Like/comment on a video 