# Waffle
Fast runtime for programming languages. Includes multiple GCs for different uses, concurrency support through lightweight green processes.

# Features
- Lightweight processes instead of native threads or green threads.
- Message based communication through processes (similar to Erlang)
- Bytecode optimizer and register allocator included
- Baseline JIT and Fusion JIT included (W.I.P)
- Multiple GC algorithms supported.

# Concurrency
Waffle uses thing called lightweight processes, you may think of it like green threads,fibers or coroutines except that lighweight processes 
does not share heap and when message from one process sent to another it's deep copied into another process heap.
When runtime initialized it spawns process scheduler with one pool for primary processes scheduling and another pool for blocking processes. 
And another thing is timeout worker that periodically checks that process received message or sleept enough time and scheduled it back to primary pool.



# Garbage collection
Waffle provides multiple GCs for your choise:
- Incremental mark&sweep (Enabled by default)

    Simple mark&sweep GC that uses freelist allocation and incremental collection, recommended for simple programs.
- Incremental generational mark&sweep

    Same as above, but also supports generations, recommended for almost every programs that doesn't make heap too fragmented.
 - Ieiunium GC 
  
    Generational GC with scavenging of nursery. This algorithm is recommended if your program may make fragmentation in heap really big. Recommended for evey kind of programs.
- Chaney's Semispace
  
    Copying GC without generations support, does very fast collectio if heap not that big.
- Mark&Sweep and maybe compact
    
    This algorithm doesn't defragment heap until fragmentation is ~60%. Recommended for every kind of programs.
- Concurrent Mark&Sweep

    GC that does small pauses to trace roots and does all work in backgound. This GC is not instantiated per process, instead it uses thread local mutators
    to properly manage all GC stuff. Recommended for all kind of programs that doesn't make heap fragmented.  
    

# TODO
- Priority based process scheduler
- Implement JIT
- Finalization support in moving collectors
