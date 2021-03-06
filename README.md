# malloc
Implement a Malloc Library
For this assignment, you have to write a malloc library that provides the following dynamic memory allocation routines (as defined in man 3 malloc):
 
> #include 

  void *malloc(size_t size);
  void free(void *ptr);
  void *calloc(size_t nmemb, size_t size);
  void *realloc(void *ptr, size_t size);
  
You have to implement a library that implements these functions to allocate and free dynamic memory.
You will need to know about memset and memcpy to implememt calloc and realloc respectively.

## Allocation
Bins: You should use separate bins for different allocation size. The idea is to then use the best-fit approach to find the right bin. You have to implement a free-list that contains a list of unallocated blocks. Once a block is allocated, it should be removed from the free-list. A call to free put a block back on the free-list for the corresponding bin. There should be four separate bins for the following allocation sizes:
8 bytes
64 bytes
512 bytes
Everything else (>512 bytes)
Allocations of size 8, 64, and 512 bytes should be done from within the heap, whereas, allocations greater than 512 bytes should be done using mmap system call. To manipulate heap, you can use the sbrk interface.

## Memory request:
If no blocks are availabe on the free-list, you must ask kernel for more memory (using sbrk() systemcall). The memory request must always be multiple of PAGE_SIZE (use the sysconf(_SC_PAGESIZE) syscall). If sbrk fails, you must set errno to ENOMEM and return NULL;
Alignment: The returned address from malloc, calloc and realloc must be aligned at 8-byte boundary.
Thread Safety
The library should be thread-safe, i.e., it should use proper locks, etc., to allow multiple threads to allocation/free memory.

## Per-Thread Malloc Arenas: 
Each thread allocates from it's own arena. That is, if there are four threads, there must be four separate arena (with their own bins).

## Support for fork
If a thread calls fork() to create a child process, can your library handle it? Try running a process with one thread calling fork() while other is calling malloc() continuously.

## Testing
The hw3 directory contains a skeleton malloc.c, Makefile, and a basic test case.

Once you have everything ready, you can use the following benchmarks/tools to test out your malloc library.

t-test1.c
Hoard benchmarks
Malloc Microbenchmark
Print Malloc Statistics
You must print the following statistics for each arena when the program calls malloc_stats() (see the manpage for more details):

1. Total size of arena allocated with sbrk/mmap
2. Total number of bins
    For each bin:
    1. Total number of blocks
    2. Used blocks
    3. Free blocks
    4. Total allocation requests
    5. Total free requests
