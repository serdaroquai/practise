#### Inverted Indexing
Full text search, 
**Tokenization** deals with John's, a state-of-the-art,  
**Normalization** U.S.A and USA should match,  
**Stemming** authorization and authorize should match,  

index the tokens to document ids  
keep tokens in memory, document ids as a **sorted** variable length list on disk.
  * so that we can use 2pointers to merge results of two words in a search phrase


#### Journaling file system (XFS)
A journaling file system is a file system that keeps track of changes not yet committed to the file system's main part by recording the intentions of such changes in a data structure known as a "journal", which is usually a circular log. In the event of a system crash or power failure, such file systems can be brought back online more quickly with a lower likelihood of becoming corrupted.

adjustable block sizes (can be suited for object storage)
With heavy sharding, append only, big files (abuse serial writing, and overcome max file count limitations).

#### Write behind
1. write to cache
2. append event to durable queue (serial write, much like Write ahead logs)
3. return ok to user
4. asnychronously process and persist the event

if db crashes events would keep being queued and service would still be available  
if queue crashes? it is highly available and replicated, write ack  
if cache crashes, the events would still end up being persisted  
how to handle cache coming back online gracefully?  

#### encoding useful info in ids

assume 1024 logical partitions -> 10 bits  
encoding this 10 bits in the id/url etc would immediately reveal which shard it is on  
add 4 bits for object type you know which table to query  

#### back of the envelope 

%80 %20 rule  
  * cache %20: %20 content generates %80 traffic  
  * 10m users, 2m users active  

**100:1 for standard read-write ratio  
**powers of 2 : 10 -> thousand, 20 -> milllion, 30 -> billion, 40 -> trillion  
**1 day has ~= 100k seconds (86400)  
**1 year has ~= 30M seconds

**disk seek = 10ms  
**ssd seek = 0.1ms  
**ram seek = 0.01ms  

**disk serial read = 100 MB/sec  
**1GBit network = 100 MB/sec  
**SSD read = 200 MB/sec
**RAM read = 2 GB/sec


assume pastebin has 1Million writes/day,  
1M/day => 12/sec (average)  
assume read write ratio 5:1, 5M reads per day.  
5M/day => 58/sec  
assume 10 Kb average for text content  
70 * 10 Kb => 1 MB/sec network overhead  
1M * 10 Kb => 10 GB storage per day => 3.6TB storage/year => 7.2TB with replication (although text could be heavily zipped)  
1M * 365 days => 365M unique ids per year => 3.6 billion for 10 years.  
base64 * 6 chars => 64^6 => (2^6)^6 => 2^30 * 2^6 => 1 billion * 64 => 64 billion unique ids  
3.6 billion ids * 36 bits ~= 140 GBit => 20GB to store ALL keys  




#### Universal scalability law
  F(N) = lambda * N / (1)      ---> would be perfectly scalable but,  
  
    * there is a *sigma percent of most tasks that can not be parallelized  
    
  F(N) = lambda * N / (1 + sigma * (N-1))  
  
    * and what if these tasks needs to coordiante (aka. cross talk)  
    
  F(N) = lambda * N / (1 + sigma * (N-1) + kappa * N * (N-1))  

### Latency numbers every programmer should know
    L1 cache reference ......................... 0.5 ns
    Branch mispredict ............................ 5 ns
    L2 cache reference ........................... 7 ns
    Mutex lock/unlock ........................... 25 ns
    Main memory reference ...................... 100 ns             
    Compress 1K bytes with Zippy ............. 3,000 ns  =   3 µs
    Send 2K bytes over 1 Gbps network ....... 20,000 ns  =  20 µs
    SSD random read ........................ 150,000 ns  = 150 µs
    Read 1 MB sequentially from memory ..... 250,000 ns  = 250 µs
    Round trip within same datacenter ...... 500,000 ns  = 0.5 ms
    Read 1 MB sequentially from SSD* ..... 1,000,000 ns  =   1 ms
    Disk seek ........................... 10,000,000 ns  =  10 ms
    Read 1 MB sequentially from disk .... 20,000,000 ns  =  20 ms
    Send packet CA->Netherlands->CA .... 150,000,000 ns  = 150 ms

Assuming ~1GB/sec SSD

    TCP connection memory read overhead 4KB (min)
    TCP connection memory write overhead 4KB (min)
    
    https://stackoverflow.com/questions/1575453/how-many-socket-connections-can-a-web-server-handle
        - http://c10m.robertgraham.com/
        -5000$ pc can easily scale to 10M connections
            - 40 gbps 32 cores 256 gb ram

### Load Balancing
* DNS level load balancing
  * hard to evict stale entries
#### L4LB
  * Have access to IP stack,
  * Can hash by source/dest x ip/port and protocol 5-tuple to (or any combination) consistently send same requests to same backend
  * They do not break the incoming client TCP connection
  * Facebook Katran, 
    * Uses XDP in linux Kernel, that is close to NIC
    * Low resource usage, lives along backends
    * State is used to drain a specific backend fast (unlike DNS drain, wait TTL until all dns lookups expire)
    * Can configure effortlessly (No modification to kernel, no restart)
    
#### L7LB
  * Kills the incoming connection
  * has access to application stack (http, cookies,etc) can leverage it to access same backends (sticky sessions)
  * Used as an SSL termination point
  * keep TCP (or other transport layer connectioned protocols) open to back ends. (TCP handshake, slowstart)
  

### Caching
* Fast since it is on memory
* Cache the queries by hashing, 
  * Hard to evict affected queries when a table column changes.
* Redis
  * TODO performance benchmark (hundreds of thousands of reads/sec)
  
* Write through (write to cache, store in db, return result)
* Write behind (write to cache, add event to queue, return result, asynchronously select and execute event)

### CAP
  * Single RDBMS, no network therefore no partition. You get both C,A
  * Any distributed system with network involved, P is there so either C or A.



### Replication
* Active replication - Push
* Passive replication - Pull
  * Data not available, read from peer then store it locally
  * Works well with timeout based caches
