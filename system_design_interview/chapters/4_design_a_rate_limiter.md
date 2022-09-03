# Chapter 4 - Design a Rate Limiter

Used to control the rate of traffic sent by a client or a service. In the HTTP world, it limits the number of client requests sent over a period of time. E.g:

- A user can write no more than 2 posts a second
- You can create a maximum of 10 accounts per day from the same IP

Benefits include:

- Protection against Denial of Service (DoS) attacks
- Reduce cost
- Prevent server overload

## Understand problem and scope

Clarifying questions:

- Client or server side
- Throttling based on user ID or IP address
- What is the scale of the system
- Will it work in a distributed environment
- Implemented as a service or in application code

Requirements:

- Accurately limit excessive requests
- Low latency - should not slow down HTTP response time
- Use as little memory as possible
- Distributed rate limiting - shared across multiple servers or processes
- Exception handling - show clear exceptions to users
- High fault tolerance - should not affect the system if it goes down

## Propose high level design

### Where to put the rate limiter?

Server side  - create rate limiter middleware whch throttles requests to your APIs. If requests aren't rate limited, they are forwarded to the API server. If they are, the rate limiter returns a `420 - Too many requests` response. 

Rate limiting is usually implemented in an API gateway. This is a fully managed service that supports rate limiting, SSL termination, IP whitelisting etc.

Answer depends on the following:

- What is the current technology stack including language, cache service?
- Identify rate limiting algorithm that fits business needs.
- If you already have microservice architecture that uses an API gateway, consider adding this there.
- Do we have enough resources to build our own rather than off the shelf?

### Algorithms for rate limiting

#### Token bucket

Each user has a bucket of tokens that fills at a preset rate up to a bucket size. Each request consumes one token. If the user does not have enough tokens, the request is dropped.

How many buckets?

- Need different buckets for different API endpoints
- If we need to throttle based on IP, each IP needs a bucket

Pros

- Allows for short bursts so long as there are tokens
- Easy to implement and memory efficient

Cons

- Must tune bucket size and refill rate

#### Leaky bucket

Similar to token bucket but requests are processed in FIFO queue. If request arrives and queue is full, request is dropped. Requests are consumed at regular intervals

Pros: 

- Memory efficient
- Suitable where stable outflow is needed

Cons:

- Bursts can fill queue with old requests and new ones will be rate limited
- Tuning bucket size and interval.

#### Fixed window counters

Divides the timeline into fixed sized windows and assigns counter for each. Each request increments the counter. Once the counter reaches a threshold, requests are dropped

Cons: 

- Spike in traffic at edge of window could let more requests go through

#### Sliding window log algorithm

Keep track of request timestamps. When a request comes in, count the requests within the time window. Add the request to the log. If the log size is lower than the quota, accept the request

Pros:

- Accuracy - requests will never exceed the quota

Cons:

- Not memory efficient

### High level architecture

1. Client sends request to rate limiting middleware
2. Middleware fetches the counter from the corresponding bucket in Redis and checks if the limit is reached or not. If the limit is reached, the request is rejected. If not, the request is forwarded to the API server.

## Design deep dive

### Rate limiting rules

Create config saved to disk and loaded up by the middleware on initialization. This allows decoupling of the configuration from the application. Example config:

```yaml
domain: auth
descriptors:
    - key: auth_type
        value: login
        rate_limit:
        unit: minute
        requests_per_unit: 5
```

### Exceeding the rate limit

In the case a rate limit is exceeded, API returns a 429 response code. We could enqueue the rejected requests to be processed later.

### Rate limiting headers
 
Allow for clients to know how close they are to their quota.

- `X-Ratelimit-Remaining`: the remaining number of allowed requests in the window
- `X-Ratelimit-Limit`: the number of calls a client can make per window
- `X-Ratelimit-Retry-After`: The number of seconds you can wait to make a request again without being throttled.


## Detailed Design

- Rules are stored on the disk, workers pull rules and store them in cache
- Client requests sent to middleware
- Middleware loads rules from the cache. Fetches counters from the Redis cache and either forwards it on or drops the request.

### In a distributed environment

#### Race condition

If two requests concurrently increment the same counter, we hace a race condition. Putting a mutex on the cache are the most obvious solution but will slow down the system. See [sorted sets in Redis](https://engineering.classdojo.com/blog/2015/02/06/rolling-rate-limiter/)

#### Synchronization issue

When multiple rate limiting servers are used, synchronization is required. This is achieved through centralized data stores like redis. This could be sharded to scale better.

#### Performance optimization

Multi-data center setup is cruical. Traffic is automatically routed to the closest edge service to reduce latency.

Synchronize data with eventual consistency model.

#### Monitoring

Gather analytics to assess if algorithm and rules are effective. Metrics such as:

- Requests accepted/rejected per resource
- Notify if there is any burst traffic
- Monitor latency of service

### Wrap up

Future considerations:

- Hard vs. soft rate limiting
- Client design
    - Retry with sufficient back-off logic
    - Use client side cache
    - Catch exceptions for graceful recovery from exceptions
