# Concurrency

* Workers can handle another customer's order while waiting for an earlier one to proceed.
* More efficient use of existing resources.

![](concurrency/concurrency.png)

During the waiting time of the first order, we can start processing the second order:

![](concurrency/concurrent_time.png)
