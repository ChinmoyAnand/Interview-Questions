 Spring / REST Interview:

1. SpringBoot caching such as in-memory caching, distributed caching, and caching annotations. 

2. How you will get to know how much time took to complete one request in spring boot.

3. What is SAGA pattern, and when would you use it? Explain in detail.

Each microservice has its own database. Some business transactions, however, span multiple service so you need a mechanism to ensure data consistency across services.
For example, lets imagine that you are building an e-commerce store where customers have a credit limit. The application must ensure that a new order will not exceed the customer’s credit limit. Since Orders and Customers are in different databases the application cannot simply use a local ACID transaction. Problem How to maintain data consistency across services? 
IMPORTANT: 2PC is not an option Implement each business transaction that spans multiple services as a saga. A saga is a sequence of local transactions. Each local transaction updates the database and publishes a message or event to trigger the next local transaction in the saga. If a local transaction fails because it violates a business rule then the saga executes a series of compensating transactions that undo the changes that were made by the preceding local transactions.


4. How would you deal with staleness of REST API cache?
Let's assume you have implemented caching of REST API in your client or web service (which depends on other webservices).

How would you ensure that the cached responses of the REST API is not stale?

Depending on the rate at which you expect the information to change, you can have different mechanism.  You have to keep additional metadata about the API response to implement caching + avoiding stale data.  Meta data that you require would be:  (a) when did you last receive the data, (b) was there an "Expires" header in the response, (c) history or rate of change of data etc.

After that, you can implement on or more of following:

1. Issue "GET + If-modified-since" (GIMS) request instead of simple GET request.
2. When your server receives down stream REST API, respond from the cache; but immediately after that, asynchronously refresh your cache from upstream server(s).
3. Issue GIMS request if you are at more than half the time between last refresh check and cache expiry.
4. If your upstream service has put too conservative Expiry header, then you can add additional logic to issue GIMS even after cache expiry using rate of change of data algorithm.  Let's say, t0 is when you received original data.  Expiry header says response will expire in 5 days.  So, on 6th day, you issue GIMS.  If it's still not expired, you assume effective expiry date = t0 + 5 days + 2 * (now - expiry as per Expires: header).  So, you won't check for 2 more days.  On 8th day, you again check issue GIMS.  If server ever returns 200 ok response, t0 resets.


------------

- Search an element in a sorted and rotated array
- [ ] What is the main objective of garbage collection?
GC 3 objectives: 1. Identify unused obj 2)clean unused obj 3)rearrange remaining obj to optimise memory usage