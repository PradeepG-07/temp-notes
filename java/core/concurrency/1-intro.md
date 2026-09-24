## Process
A process is an instance of a program that is currently executing.
A process has its own address space (heap, code segment, data segment, etc.). Each thread within the process has its own stack and program counter.
## Thread
A thread is the smallest unit of execution within a process. Often called as *lightweight* process.
There can be multiple threads within a single process.
Each thread has its own stack and program counter, while sharing the heap and code area of the process with other threads.

## Differences between thread and process

| S.no | Thread                                                          | Process                                                       |
|------|-----------------------------------------------------------------|---------------------------------------------------------------|
| 1    | Part of a process                                               | Program in execution                                          |
| 2    | Shares process memory with other threads                        | Has its own address space                                     |
| 3    | Shares data with other threads                                  | Processes do not share data by default.                       |
| 4    | Takes less time to create and <br>terminate compared to process | Takes more time to create and terminate<br>compared to thread |

## Parallelism
Parallelism is the simultaneous execution of multiple tasks on multiple CPU cores.
**Example:** Consider there are 4 CPU cores and there are 4 processes which are working on different aspects.
Here at any given point of time these 4 processes will always be running on each CPU core parallely without any interruption.
## Concurrency
Concurrency is the ability of a system to make progress on multiple tasks during overlapping time periods. This is often achieved using context switching, where the CPU rapidly switches between tasks.
**Example:** Consider there is only 1 CPU core and there are 4 processes that needs to be executed.
Then OS uses a technique called *context switching*, which makes the user feel that all 4 processes are running simultaneously, but they are not.
## True Parallelism
True parallelism occurs when two or more threads/processes are executing simultaneously on different CPU cores.