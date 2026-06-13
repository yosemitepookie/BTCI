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
1. 