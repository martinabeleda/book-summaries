# Chapter 2 - Back of the Envelope Estimation

> Back of the envelope calculations are estimates you create using a combination of thought experiments and
> common performance numbers to get a good feel for which designs will meet your requirements

## Power of Two

It is critical to know the data volume unit using the power of 2. A byte is a sequence of 8 bits. An ASCII
character yses one byte of memory.

| Power | Approximate value | Full name  | Short name |
|-------|-------------------|------------|------------|
| 10    | 1 Thousand        | 1 Kilobyte | 1 KB       |
| 20    | 1 Million         | 1 Megabyte | 1 MB       |
| 30    | 1 Billion         | 1 Gigabyte | 1 GB       |
| 40    | 1 Trillion        | 1 Terabyte | 1 TB       |
| 50    | 1 Quadrillion     | 1 Petabyte | 1 PB       |

## Latency numbers

The visualization below gives a good idea of the relative fastness and slowness of common computer
operations:

![Latency visualization](https://i.imgur.com/k0t1e.png)

We can make the following conclusions

- Memory is fast, disk is slow
- Avoid disk seeks if possible
- Simple compression algorithms are fast
- Compress data before sending it over the internet if possible
- Data centres are usually in different regions, and it takes time to send data between them

## Availability numbers

**High availability:**

> The ability of a system to be continuously operational for a desirably long period of time. 
> It is measured as a percentage with 100% means a service that has 0 downtime. Most services 
> fall between 99% and 100%.

A service level agreement (SLA) is a commonly used term for service providers.

| Availability level | Downtime     |                 |               |              |              |              |
|--------------------|--------------|-----------------|---------------|--------------|--------------|--------------|
|                    | **per year** | **per quarter** | **per month** | **per week** | **per day**  | **per hour** |
| 99%                | 3.65 days    | 21.6 hours      | 7.2 hours     | 1.68 hours   | 14.4 minutes | 36 seconds   |
| 99.5%              | 1.83 days    | 10.8 hours      | 3.6 hours     | 50.4 minutes | 7.20 minutes | 18 seconds   |
| 99.9%              | 8.76 hours   | 2.16 hours      | 43.2 minutes  | 10.1 minutes | 1.44 minutes | 3.6 seconds  |
| 99.95%             | 4.38 hours   | 1.08 hours      | 21.6 minutes  | 5.04 minutes | 43.2 seconds | 1.8 seconds  |
| 99.99%             | 52.6 minutes | 12.96 minutes   | 4.32 minutes  | 60.5 seconds | 8.64 seconds | 0.36 seconds |
| 99.999%            | 5.26 minutes | 1.30 minutes    | 25.9 seconds  | 6.05 seconds | 0.87 seconds | 0.04 seconds |

## Example: Estimate Twitter QPS and storage requirements

### Assumptions

- 300 million monthly active users
- 50% of daily users
- Users post 2 tweets per day on average
- 10% of tweets contain media
- Data is stored for 5 years

### Estimations

#### Query per second (QPS)

- Daily active users (DAU) = 300 million * 0.5 = 150 million
- Tweets QPS = 150 million * 2 tweets / 24 hours / 3600 seconds = ~3500
- Peak QPS = 2 * QPS = ~7000

#### Media storage

- Average tweet size:
    - `tweet_id`: 64 bytes
    - `text`: 140 bytes
    - `media`: 1 MB
- Media storage: 150 million * 2 * 10% * 1MB = 30 TB per day
- 5-year media storage: 30 TB * 365 * 5 = ~55 PB
