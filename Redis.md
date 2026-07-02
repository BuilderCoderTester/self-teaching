# Redis Notes

## Table of Contents

1. [Installing Redis on Windows using WSL](#installing-redis-on-windows-using-wsl)
2. [Starting Redis Server](#starting-redis-server)
3. [Checking Redis Server Status](#checking-redis-server-status)
4. [Redis Strings](#1-redis-strings)
5. [Redis Lists](#2-redis-lists)
6. [Redis Sets](#3-redis-sets)
7. [Redis HyperLogLog](#4-redis-hyperloglog)

---

# Installing Redis on Windows using WSL

## Step 1: Install WSL

Open **PowerShell** or **Command Prompt** as Administrator and run:

```powershell
wsl --install
```

This command:

- Enables the Windows Subsystem for Linux (WSL)
- Enables the Virtual Machine Platform feature
- Downloads the latest WSL version
- Installs Ubuntu by default
- Sets WSL2 as the default version

Restart your computer if prompted.

---

## Step 2: Install Redis

Open the Ubuntu terminal and run:

```bash
sudo apt update
sudo apt install redis-server -y
```

---

# Starting Redis Server

Start Redis using:

```bash
sudo service redis-server start
```

or

```bash
sudo systemctl start redis-server
```

---

# Checking Redis Server Status

Open Redis CLI:

```bash
redis-cli
```

Then execute:

```redis
PING
```

Output:

```text
PONG
```

If Redis returns **PONG**, the Redis server is running successfully.

---

# 1. Redis Strings

Strings are the simplest Redis data type. They store values as **key-value pairs**.

Example:

```text
name -> Anurag
age  -> 24
city -> Kolkata
```

## String Commands

### SET

Stores a value.

```redis
SET name Shabir
```

Output

```text
OK
```

---

### GET

Retrieves a value.

```redis
GET name
```

Output

```text
"Shabir"
```

---

### GETRANGE

Returns a substring.

```redis
SET email shabir@gmail.com

GETRANGE email 0 5
```

Output

```text
"shabir"
```

---

### MSET

Stores multiple key-value pairs.

```redis
MSET language English technology Redis country India
```

Output

```text
OK
```

---

### MGET

Retrieves multiple values.

```redis
MGET language technology country
```

Output

```text
1) "English"
2) "Redis"
3) "India"
```

---

### STRLEN

Returns string length.

```redis
SET technology Redis

STRLEN technology
```

Output

```text
5
```

---

### INCR

Increment an integer by 1.

```redis
SET count 10

INCR count
```

Output

```text
11
```

---

### DECR

Decrement an integer by 1.

```redis
SET count 10

DECR count
```

Output

```text
9
```

---

### INCRBY

Increment by a specified amount.

```redis
SET count 10

INCRBY count 5
```

Output

```text
15
```

---

### DECRBY

Decrease by a specified amount.

```redis
SET count 20

DECRBY count 8
```

Output

```text
12
```

---

### INCRBYFLOAT

Increment floating-point numbers.

```redis
SET pi 3.14

INCRBYFLOAT pi 0.001
```

Output

```text
3.141
```

---

### EXPIRE

Sets expiration time.

```redis
SET session user123

EXPIRE session 10
```

The key will automatically be deleted after **10 seconds**.

---

### TTL

Checks remaining time.

```redis
TTL session
```

Output

```text
8
```

(Example output)

---

### SETEX

Stores a value with expiration.

```redis
SETEX token 30 abc123
```

The key expires after **30 seconds**.

---

## Summary

| Command | Description |
|----------|-------------|
| SET | Store value |
| GET | Retrieve value |
| GETRANGE | Get substring |
| MSET | Store multiple values |
| MGET | Retrieve multiple values |
| STRLEN | Length of string |
| INCR | Increase integer |
| DECR | Decrease integer |
| INCRBY | Increase by amount |
| DECRBY | Decrease by amount |
| INCRBYFLOAT | Increase float |
| EXPIRE | Set expiration |
| TTL | Check remaining time |
| SETEX | Store value with expiration |

---

# 2. Redis Lists

Lists are ordered collections of strings.

Redis lists behave like linked lists.

Example:

```text
India
USA
Germany
Australia
```

---

## LPUSH

Insert at the left.

```redis
LPUSH country India
```

---

```redis
LPUSH country USA
```

List becomes:

```text
USA
India
```

---

## RPUSH

Insert at the right.

```redis
RPUSH country Australia
```

List:

```text
USA
India
Australia
```

---

## LPUSHX

Push only if the list exists.

```redis
LPUSHX movies Inception
```

---

## RPUSHX

Push to the right only if the list exists.

```redis
RPUSHX movies Interstellar
```

---

## LRANGE

Retrieve list elements.

```redis
LRANGE country 0 -1
```

Output

```text
1) "USA"
2) "India"
3) "Australia"
```

---

## LLEN

Length of list.

```redis
LLEN country
```

Output

```text
3
```

---

## LPOP

Remove first element.

```redis
LPOP country
```

Output

```text
"USA"
```

---

## RPOP

Remove last element.

```redis
RPOP country
```

Output

```text
"Australia"
```

---

## LSET

Replace value at an index.

```redis
LSET country 0 Germany
```

---

## LINSERT

Insert before or after a value.

```redis
LINSERT country BEFORE Germany NewZealand
```

Result

```text
NewZealand
Germany
India
```

---

## LINDEX

Retrieve value by index.

```redis
LINDEX country 1
```

Output

```text
"Germany"
```

---

## SORT

Sort the list.

```redis
SORT country ALPHA
```

Output

```text
Australia
Germany
India
USA
```

---

## BLPOP

Blocking left pop.

```redis
BLPOP movies 15
```

Waits for up to **15 seconds** if the list is empty.

---

## BRPOP

Blocking right pop.

```redis
BRPOP movies 15
```

Waits for up to **15 seconds**.

---

## Summary

| Command | Description |
|----------|-------------|
| LPUSH | Push left |
| RPUSH | Push right |
| LPUSHX | Push left if exists |
| RPUSHX | Push right if exists |
| LRANGE | Retrieve range |
| LLEN | List length |
| LPOP | Remove first |
| RPOP | Remove last |
| LSET | Update index |
| LINSERT | Insert before/after |
| LINDEX | Retrieve by index |
| SORT | Sort list |
| BLPOP | Blocking left pop |
| BRPOP | Blocking right pop |

---

# 3. Redis Sets

Sets store **unique unordered values**.

Duplicate values are automatically ignored.

---

## SADD

Add members.

```redis
SADD technology Java Redis NodeJS AWS
```

---

## SMEMBERS

Retrieve all members.

```redis
SMEMBERS technology
```

Output

```text
1) Java
2) Redis
3) NodeJS
4) AWS
```

---

## SCARD

Count members.

```redis
SCARD technology
```

Output

```text
4
```

---

## SISMEMBER

Check if a member exists.

```redis
SISMEMBER technology Java
```

Output

```text
1
```

If not present:

```text
0
```

---

## SDIFF

Difference between sets.

```redis
SADD frontend HTML CSS JavaScript

SDIFF technology frontend
```

Output

```text
Redis
NodeJS
AWS
Java
```

---

## SDIFFSTORE

Store difference.

```redis
SDIFFSTORE backend technology frontend
```

---

## SINTER

Common members.

```redis
SINTER technology frontend
```

Output

```text
JavaScript
```

(if present in both)

---

## SINTERSTORE

Store intersection.

```redis
SINTERSTORE common technology frontend
```

---

## SUNION

Union of sets.

```redis
SUNION technology frontend
```

Output

```text
Java
Redis
NodeJS
AWS
HTML
CSS
JavaScript
```

---

## SUNIONSTORE

Store union.

```redis
SUNIONSTORE allskills technology frontend
```

---

## Summary

| Command | Description |
|----------|-------------|
| SADD | Add members |
| SMEMBERS | Retrieve all members |
| SCARD | Count members |
| SISMEMBER | Check membership |
| SDIFF | Difference |
| SDIFFSTORE | Store difference |
| SINTER | Intersection |
| SINTERSTORE | Store intersection |
| SUNION | Union |
| SUNIONSTORE | Store union |

---

# 4. Redis HyperLogLog

HyperLogLog is a probabilistic data structure used to estimate the number of **unique elements (cardinality)** in a very large dataset while consuming very little memory.

Unlike a Redis Set, HyperLogLog **does not store every element**. Instead, it uses mathematical algorithms to estimate the number of unique values.

Typical use cases include:

- Counting website visitors
- Counting unique IP addresses
- Counting search queries
- Counting unique email addresses
- Counting unique users
- Counting unique locations

---

## PFADD

Adds elements to a HyperLogLog.

```redis
PFADD hll_visitors 192.168.1.1 192.168.1.2
```

---

## PFCOUNT

Returns the estimated number of unique elements.

```redis
PFCOUNT hll_visitors
```

Example output

```text
2
```

It can also count across multiple HyperLogLogs.

```redis
PFCOUNT hll_jan hll_feb
```

---

## PFMERGE

Merge multiple HyperLogLogs.

```redis
PFMERGE total_visitors hll_jan hll_feb
```

Then:

```redis
PFCOUNT total_visitors
```

Returns the estimated total number of unique visitors across both months.

---

## Summary

| Command | Description |
|----------|-------------|
| PFADD | Add elements |
| PFCOUNT | Count unique elements |
| PFMERGE | Merge HyperLogLogs |

---

# Quick Comparison

| Data Type | Stores | Ordered | Duplicate Allowed | Common Use Case |
|------------|--------|---------|-------------------|-----------------|
| String | Single value | N/A | Yes | User info, counters, cache |
| List | Collection | Yes | Yes | Queue, timeline, recent items |
| Set | Collection | No | No | Tags, unique users, skills |
| HyperLogLog | Estimated unique count | N/A | N/A | Visitor counting, analytics |

---

# Key Takeaways

- **Strings** are used for simple key-value storage.
- **Lists** maintain insertion order and are ideal for queues and stacks.
- **Sets** store unique values and support set operations like union and intersection.
- **HyperLogLog** provides memory-efficient approximate counting of unique elements.
- Redis operations are **atomic**, ensuring consistency even with concurrent clients.

# Redis Transactions

Redis Transactions allow you to execute multiple commands as a single unit of work. Transactions ensure that all queued commands are executed sequentially without interruption from other clients.

## Key Features

- Executes multiple commands in a single transaction.
- Provides **atomic execution**, meaning queued commands are executed together.
- Commands are queued until `EXEC` is called.
- Supports conditional execution using `WATCH`.
- Transactions can be cancelled using `DISCARD`.

---

## Transaction Commands

### MULTI

Starts a transaction. All subsequent commands are queued instead of being executed immediately.

```redis
MULTI
```

Output

```text
OK
```

---

### Queue Commands

Once inside transaction mode, commands are queued.

```redis
SET name Shabir
SET A 1
SET B 2
```

Output

```text
QUEUED
QUEUED
QUEUED
```

---

### EXEC

Executes all queued commands.

```redis
EXEC
```

Output

```text
1) OK
2) OK
3) OK
```

---

### DISCARD

Cancels the transaction and removes all queued commands.

```redis
MULTI

SET name Redis

DISCARD
```

Output

```text
OK
```

No commands are executed.

---

### WATCH

Monitors one or more keys before executing a transaction.

If another client modifies any watched key before `EXEC` is called, the transaction is aborted.

```redis
WATCH balance

MULTI

DECRBY balance 100

EXEC
```

If another client changes `balance` before `EXEC`, Redis returns:

```text
(nil)
```

The transaction is cancelled.

---

## Complete Transaction Example

```redis
MULTI

SET name Shabir
SET A 1
SET B 2

EXEC
```

Output

```text
1) OK
2) OK
3) OK
```

---

## Transaction Workflow

```text
MULTI
   │
   ▼
Queue Commands
   │
   ▼
EXEC
   │
   ▼
All Commands Executed
```

Or

```text
MULTI
   │
   ▼
Queue Commands
   │
   ▼
DISCARD
   │
   ▼
Transaction Cancelled
```

---

## Summary

| Command | Description |
|----------|-------------|
| MULTI | Starts a transaction |
| EXEC | Executes queued commands |
| DISCARD | Cancels the transaction |
| WATCH | Watches keys for modifications |

---

# Redis Pub/Sub (Publisher-Subscriber)

Redis Pub/Sub is a real-time messaging system where clients communicate through **channels**.

- **Publishers** send messages to channels.
- **Subscribers** listen to channels.
- Messages are delivered instantly to all subscribed clients.

---

## Pub/Sub Architecture

```text
           Publisher
                │
         PUBLISH news
                │
        ----------------
        |              |
 Subscriber 1    Subscriber 2
  SUBSCRIBE       PSUBSCRIBE
     news            news*
```

---

## Pub/Sub Commands

### SUBSCRIBE

Subscribe to a specific channel.

```redis
SUBSCRIBE news
```

Output

```text
Subscribed to channel "news"
```

The client remains listening until the connection is closed.

---

### PUBLISH

Sends a message to a channel.

```redis
PUBLISH news "Breaking News!"
```

Output

```text
2
```

The returned number indicates how many subscribers received the message.

---

### PSUBSCRIBE

Subscribe using a pattern.

```redis
PSUBSCRIBE news*
```

This subscriber receives messages from channels such as:

- news
- news_today
- news_update
- news_world

---

## Wildcards

### *

Matches zero or more characters.

Example

```text
news*
```

Matches:

```text
news
news1
news_today
news_update
```

---

### ?

Matches exactly one character.

Pattern

```text
h?llo
```

Matches

```text
hello
hallo
```

Does **not** match

```text
heello
```

---

### [ ]

Matches one character from the specified set.

Pattern

```text
b[ai]ll
```

Matches

```text
ball
bill
```

Does **not** match

```text
bell
```

---

# Pub/Sub Management Commands

## PUBSUB CHANNELS

Lists all active channels that currently have subscribers.

```redis
PUBSUB CHANNELS
```

Example Output

```text
news
sports
chat
```

---

## PUBSUB NUMSUB

Shows the number of subscribers for a channel.

```redis
PUBSUB NUMSUB news
```

Example Output

```text
news
2
```

---

## PUBSUB NUMPAT

Returns the total number of pattern-based subscriptions.

```redis
PUBSUB NUMPAT
```

Example Output

```text
3
```

---

# Complete Pub/Sub Example

### Client 1 (Subscriber)

```redis
SUBSCRIBE news
```

---

### Client 2 (Pattern Subscriber)

```redis
PSUBSCRIBE news*
```

---

### Client 3 (Publisher)

```redis
PUBLISH news "Breaking News!"
```

---

### Result

Both subscribers receive:

```text
Breaking News!
```

---

# Pub/Sub Workflow

```text
Client 1
SUBSCRIBE news
        │
        ▼

      Channel (news)

        ▲
        │

PUBLISH "Breaking News!"

        │
        ▼

Client 2
PSUBSCRIBE news*
```

---

## Summary

| Command | Description |
|----------|-------------|
| SUBSCRIBE | Subscribe to a channel |
| PUBLISH | Publish a message |
| PSUBSCRIBE | Subscribe using patterns |
| PUBSUB CHANNELS | List active channels |
| PUBSUB NUMSUB | Number of subscribers |
| PUBSUB NUMPAT | Number of pattern subscriptions |

---

# Key Takeaways

- **Transactions** execute multiple commands together using `MULTI` and `EXEC`.
- **WATCH** enables optimistic locking by monitoring keys for changes.
- **DISCARD** cancels queued commands before execution.
- **Pub/Sub** enables real-time communication between publishers and subscribers.
- **PSUBSCRIBE** allows wildcard subscriptions to multiple channels.
- **PUBSUB** commands help monitor active channels and subscribers.
