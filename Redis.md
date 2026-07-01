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