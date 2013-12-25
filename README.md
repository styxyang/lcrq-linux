lcrq-linux
==========

Modify original implementation to suit linux environment

This is a modified version of LCRQ by Adam Morrison and Yehuda Afek.
If you are interested in the idea and internals, please refer to paper [Fast Concurrent Queues for x86 Processors](http://dl.acm.org/citation.cfm?id=2442527) published at _PPoPP 2013_ by Adam Morrison, Yehuda Afek [Download pdf](http://www.cs.tau.ac.il/~adamx/g.php?link=http://www.cs.tau.ac.il/~adamx/ppopp2013-x86queues.pdf)

LCRQ is a multi-producer multi-consumer concurrent queue. And the key point of concurrent queue is that to what extent the semantics of _order_ is guarenteed. Actually concurrent queues doesn't provide sequential consistency. The order of elements being dequeued and enqueued is guarenteed but the order of threads getting the value of the dequeued elements or filling the queue slot is not guarenteed

My port still contains bugs or performance issues. Use it with caution.



Copyright (c) 2013, Adam Morrison and Yehuda Afek.
All rights reserved.

Published under 3-clause BSD License.
