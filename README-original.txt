
LCRQ: FAST CONCURRENT QUEUE FOR x86 PROCESSORS
==============================================

Authors: Adam Morrison <adamx@tau.ac.il>
         Yehuda Afek   <afek@tau.ac.il>
URL:     <http://mcg.cs.tau.ac.il/projects/lcrq>


About the package
-----------------
This package contains the source code of the LCRQ concurrent queue
implementation used in the paper ``Fast Concurrent Queues for x86
Processors'' by Adam Morrison and Yehuda Afek [1].

LCRQ stands for ``list of concurrent ring queues''.  The LCRQ is a
a high-performance nonblocking multi-producer/multi-consumer (MPMC)
linearizable FIFO queue.  By nonblocking we refer to the property
that some thread is always able to complete an operation [3].
(This property is sometimes called lock-freedom.)


Contents of package
-------------------
We are providing a complete benchmark program which is meant to be
plugged into the ``sim-universal-construction'' benchmark framework
used in Panagiota Fatourou and Nikolaos D. Kallimanis's paper
``Revisiting the Combining Synchronization Technique'' [2].

It is straightforward to extract only the queue implementation from
the provided benchmark program.  You simply need to provide your own
implementations of atomic memory operations, memory allocation, and
so on.

We are also providing a patch that improves the performance of the 
CC-Queue and H-Queue implementations in the framework by fixing a
memory leak,  With this patch, removed queue nodes are placed back
in the private memory pools.  As a result, most allocations can be
satisfied from the pools and the number of malloc() calls drops
considerably.  


File index:

  lcrq.c
    - The benchmark program that contains the LCRQ implementation.

  0001-Fix-memory-leaks-in-CC-Queue-and-H-Queue.patch
    - The CC-Queue and H-Queue memory leak fix.


Memory reclamation
------------------
The queue implementation can be integrated with a memory reclamation
system based on hazard pointers [4].

While we do not provide a complete hazard pointers implementation
(e.g., the memory management code which scans threads' hazard pointers
to decide if freeing memory is safe), the code contains place-holders
for where hazard pointers need to be set.  There is a configuration
option (see below) which enables these place-holders.

Thus, if you have a hazard pointers implementation it should be
straightforward to convert LCRQ to use it.


Configurations knobs
--------------------
There are 3 configuration definitions you can change in lcrq.c:

RING_SIZE
    - The size of the ring in each node.  We are currently using
      a fixed size for all nodes.  In the future, to use memory
      more efficiently, we may switch to a dynamic algorithm
      that starts with a small ring and increases it when there is
      contention.

RING_STATS
    - Define this to collect and display algorithmic statistics.
      (Off by default.)

HAVE_HPTRS
    - Set this to have queue operation set hazard pointers.
      (Off by default.)


Requirements
------------
As mentioned above, you need Fatourou and Kallimanis's framework:

    http://code.google.com/p/sim-universal-construction/

We have used version 1.0.1:

    https://sim-universal-construction.googlecode.com/files/synch1.0.1.zip

IMPORTANT:  You will need to adjust the framework's thread binding code
to the topology of your machine.


References
----------
[1] Adam Morrison and Yehuda Afek.
    ``Fast Concurrent Queues for x86 Processors.''
    Proceedings of the 18th ACM SIGPLAN Symposium on Principles and Practice of Parallel Programming (PPoPP 2013).

[2] Panagiota Fatourou and Nikolaos D. Kallimanis.
    ``Revisiting the Combining Synchronization Technique.''
    Proceedings of the 17th ACM SIGPLAN Symposium on Principles and Practice of Parallel Programming (PPoPP 2012).

[3] Maurice Herlihy.
    ``Wait-free synchronization.''
    ACM Transactions on Programming Languages and Systems (TOPLAS), January 1991.

[4] Maged M. Michael.
    ``Hazard pointers: Safe memory reclamation for lock-free objects.''
    IEEE Transactions on Parallel and Distributed System, June 2004.

