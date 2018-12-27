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
* Cache the queries by hashing, 
  * Hard to evict affected queries when a table column changes.
 *
