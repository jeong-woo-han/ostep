# OSTEP의 내용 중 기억에 남는 부분을 요약하였습니다.

## Part 1. Virtualization

* `fork()`, `wait()`, `exec()': `fork()`와 `exec()`을 분리하는 이유는 exec() 전에 환경을 바꾸고, 메모리 I/O를 줄이기 위함.
* Scheduling: FIFO, SJF, STCF, Round Robin, MLFQ, Ticket, Ticket을 Niceness로 가중하는 방법
* Segmentation의 적용, base와 bound register
* Page table을 multi-level로 하여 PDE-PTE 구조
* Cache Replacement Policies: FIFO, Random, LRU, Approximating LRU

## Part 2. Concurrency

* 기초적인 memory 구현: Process의 메모리 공간 구성, Address Translation, Free-Space Management, Paging
* Thread API는 shared data 접근이 꼬이는 것이 문제이므로 Lock, Condition Variables로 해결
* Spinning Locks: Test-And-Set, Compare-And-Swap, LL-SC, Fetch-And-Add
* Nonspinning Locks: Yield, Queue(guard)
* Condition Variables: 잠든 thread에게 준비되었다고 signal하기 위해 만듦. Producer/Consumer
* Semaphore: 둘을 합침. `wait()`, `post()`로 작동.
* Deadlock 문제: 서로가 차지한 lock이 서로의 조건을 훼방하여 어떤 lock도 풀리지 않는 상태
* Concurrency의 문제 상황 중 Non-Deadlock Bugs: Atomicity-Violation Bugs, Order-Violation Bugs
* Concurrency의 문제 상황 중 Deadlock Bugs: Mutual Exclusion, Hold-and-wait, No preemption, Circular wait의 조건이 만족되어야 발생
* Event-based concurrency: Thread가 아니라 event 기반으로 구현. 비동기식 I/O가 필요해짐.

## Part 3. Persistence

* RAID
* File, Directory, File Descriptor(0~2 예약)
* Disk 안에는 inode table, 각 디렉토리 아래에 directory entry 구조체도 있고 프로세스가 죽어도 남아있음. 그러나 open file table이나 fd는 프로세스의 것, 프로세스가 죽으면 죽음.
* Hard Links, Symbolic Links
* Very Simple FS -> FFS
* Crash 발생 대책: File System Checker, Journaling
* Log-Structured File System(LFS): Log 그 자체를 write하며, 절대 overwrite하지 않음. imap, checkpoint(CR) 도입. segment summary(ss)에 block liveness 기록.
* SSD의 disk failure와 해결
* Distributed System: NFS(block 단위, stateless), AFS(whole file caching, stateful)
