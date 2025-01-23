
# Introduction to Redis and Basic Commands

This README provides an overview of how to use Redis, a key-value database often used for high-performance operations. It explains basic Redis commands, working with keys, managing persistence, and utilizing data structures like lists and sets. Redis is well-documented, and this guide serves as an introductory reference.

---

## Table of Contents
1. [What is Redis?](#what-is-redis)
2. [Prerequisites](#prerequisites)
3. [Installation](#installation)
4. [Basic Commands](#basic-commands)
5. [Working with Lists](#working-with-lists)
6. [Working with Sets](#working-with-sets)
7. [Data Persistence](#data-persistence)
8. [Further Learning](#further-learning)

---

## What is Redis?
Redis is an in-memory key-value database that supports a wide variety of data structures such as strings, lists, sets, hashes, and more. It is commonly used for caching, real-time analytics, and other applications that require fast, ephemeral storage. Redis can also persist data on disk for longer-term storage.

---

## Prerequisites
- Basic knowledge of databases and the command line.
- Redis installed on your machine.

---

## Installation
### Installing Redis on Linux:
1. Update package lists:
   ```bash
   sudo apt update
   ```
2. Install Redis:
   ```bash
   sudo apt install redis-server
   ```
3. Start the Redis service:
   ```bash
   sudo systemctl start redis
   ```

### Installing Redis on macOS (with Homebrew):
1. Install Redis:
   ```bash
   brew install redis
   ```
2. Start Redis:
   ```bash
   redis-server
   ```

### Verify Installation:
Use the command:
```bash
redis-cli ping
```
It should return:
```
PONG
```

---

## Basic Commands

### Starting the Server
To start Redis, run:
```bash
redis-server
```
To interact with it, use the Redis CLI:
```bash
redis-cli
```

### Defining and Managing Keys
1. **Set a key-value pair**:
   ```bash
   SET mykey "hello"
   ```
2. **Get a value**:
   ```bash
   GET mykey
   ```
3. **Delete a key**:
   ```bash
   DEL mykey
   ```
4. **Increment a key**:
   ```bash
   INCR counter
   ```
5. **Decrement a key**:
   ```bash
   DECR counter
   ```

---

## Working with Lists
Redis lists allow you to store ordered collections of strings.

1. **Add elements to a list**:
   - Left push:
     ```bash
     LPUSH mylist "item1"
     ```
   - Right push:
     ```bash
     RPUSH mylist "item2"
     ```
2. **Retrieve elements from a list**:
   ```bash
   LRANGE mylist 0 -1
   ```
   (This retrieves all elements.)
3. **Pop elements from a list**:
   - Left pop:
     ```bash
     LPOP mylist
     ```
   - Right pop:
     ```bash
     RPOP mylist
     ```

---

## Working with Sets
Redis sets store unique elements.

1. **Add elements to a set**:
   ```bash
   SADD myset "item1" "item2" "item3"
   ```
2. **Retrieve all elements from a set**:
   ```bash
   SMEMBERS myset
   ```
3. **Remove an element from a set**:
   ```bash
   SREM myset "item1"
   ```
4. **Perform union operations between sets**:
   ```bash
   SUNION set1 set2
   ```

---

## Data Persistence
Redis stores data in memory but can persist it to disk. To configure persistence:
1. Open the Redis configuration file:
   ```bash
   sudo nano /etc/redis/redis.conf
   ```
2. Modify the `save` directive to define when data should be saved (e.g., after a specific number of changes or a time interval).

### Example:
```conf
save 900 1
save 300 10
save 60 10000
```
This saves the dataset if:
- 1 change occurs within 15 minutes.
- 10 changes occur within 5 minutes.
- 10,000 changes occur within 1 minute.

Restart Redis after making changes:
```bash
sudo systemctl restart redis
```

---

## Further Learning
Redis offers extensive documentation on its official site. You can explore commands, configurations, and advanced use cases here:
[Redis Documentation](https://redis.io/docs)

### Example Searches:
- To learn about lists:
  ```bash
  HELP LIST
  ```
- To explore commands for sets:
  ```bash
  HELP SET
  ```

---

Redis is a powerful tool that can enhance the performance of your applications. This guide covers only the basics—explore its full capabilities on the [Redis website](https://redis.io/).
