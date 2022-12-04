# Operating-System Terms

## Overview

**Operating System:** A program that manages a computer’s hardware, provides a basis for application programs, and acts as an intermediary between the computer user and the computer hardware.  

**Kernel:** The operating system component running on the computer at all times after system boot.

**Bootstrap:** The set of steps taken at computer power-on to bring the system to full operation.

**Bootstrap Loader:** The small program that loads the kernel as part of the bootstrap procedure.

**Boot Block:** A block of code stored in a specific location on disk with the instructions to boot the kernel stored on that disk.

**Firmware:** Software stored in ROM or EPROM for booting the system and managing low level hardware (e.g., bootstrap).

**Device Controller:** The I/O managing processor within a device.

**Device Driver:** An operating system component that provides uniform access to various devices and manages I/O to those devices.

**Interrupt:** A hardware mechanism that enables a device to notify the CPU that it needs attention.

**Interrupt Service Routine (ISR):** An operating system routine that is called when an interrupt signal is received.

**Interrupt Vector:** A kernel memory data structure that holds the addresses of the interrupt service routines for the various devices.

**Trap (Exception):** A software interrupt. The interrupt can be caused either by an error (e.g., division by zero or invalid memory access) or by a specific request from a user program that an operating-system service be performed.

**System Call:** Software-triggered interrupt allowing a process to request a kernel service.

**Application Programming Interface (API):** A set of commands, functions, and other tools that can be used by a programmer in developing a program.

**Polling:** An I/O loop in which an I/O thread continuously reads status information waiting for I/O to complete.

**Synchronous:** In interprocess communication, a mode of communication in which the sending process is blocked until the message is received by the receiving process or by a mailbox and the receiver blocks until a message is available. In I/O, a request that does not return until the I/O completes.  

**Asynchronous:** In I/O, a request that executes while the caller continues execution.

**Device-Status Table:** A kernel data structure for tracking the status and queues of operations for devices.

**Direct Memory Access (DMA):** A resource-conserving and performance-improving operation for device controllers allowing devices to transfer large amounts of data directly to and from main memory.

**Cache:** A temporary copy of data stored in a reserved memory area to improve performance.

**Cache Coherency:** The coordination of the contents of caches such that an update to a value stored in one cache is immediately reflected in all other caches that hold that value.

**Buffer:** A memory area that stores data being transferred (e.g., between two devices or between a device and a process).

**Spool:** A buffer that holds output for a device (such as a printer) that cannot accept interleaved data streams.

**Multiprogramming:** A technique that increases CPU utilization by organizing jobs (code and data) so that the CPU always has a job to execute.

**Multitasking:** The concurrent performance of multiple jobs. A CPU executes multiple jobs by switching among them, but the switches occur so frequently that users can interact with the processes.

**Multiprocessor Systems:** Systems that have two or more hardware processors (CPU cores) in close communication, sharing the computer bus and sometimes the clock, memory, and peripheral devices.  

**Response Time:** The amount of time it takes the system to respond to user action.

**User Mode:** A CPU mode for executing user processes in which some instructions are limited or not allowed.

**Kernel Mode:** A CPU mode in which all instructions are enabled. The kernel runs in this mode.

**Mode Bit:** A CPU status bit used to indicate the current mode: kernel (0) or user (1).

**Timer:** A hardware component that can be set to interrupt the computer after a specified period.

**Protection:** A category of system calls. Any mechanism for controlling the access of processes or users to the resources defined by a computer system.

**Security:** The defense of a system from external and internal attacks. Such attacks include viruses
and worms, denial-of-service attacks, identity theft, and theft of service.

**User Interface (UI):** A method by which a user interacts with a computer.

**Command-Line Interface (CLI):** A method of giving commands to a computer based on a text input device (such as a keyboard).

**Graphical User Interface (GUI):** A computer interface comprising a window system with a pointing device to direct I/O, choose from menus, and make selections and usually, a keyboard to enter text.

**Batch Interface:** A method for giving commands to a computer in which commands are entered into files, and the files are executed, without any human interaction.

**System Program:** A program associated with the operating system but not necessarily part of the kernel.

**Monolithic:** In kernel architecture, describes a kernel without structure (such as layers or modules).

**Layered Approach:** A kernel architecture in which the operating system is separated into a number of layers (levels); typically, the bottom layer (layer 0) is the hardware, and the highest (layer N) is the user interface.

**Microkernel:** An operating-system structure that removes all nonessential components from the kernel and implements them as system and user-level programs.

**Virtual Machine (VM):** The abstraction of hardware allowing a virtual computer to execute on a physical computer. Multiple virtual machines can run on a single physical machine (and each can have a different operating system).

**Java Virtual Machine (JVM):** In programming-environment virtualization, the process that implements the Java language and allows execution of Java code.



## Process Management

**Process:** A program loaded into memory and executing.

**Program Counter:** A CPU register indicating the main memory location of the next instruction to load and execute.

**Process Control Block (PCB):** A per-process kernel data structure containing many pieces of information associated with the process.

**Ready Queue:** The set of processes ready and waiting to execute.

**Device Queue:** The list of processes waiting for a particular I/O device.

**Job Scheduling (Long-Term Scheduling):** The task of choosing which jobs to load into memory and execute.

**CPU scheduler (Short-Term Scheduler):** Kernel routine that selects a thread from the threads that are ready to execute and allocates a core to that thread.

**Swapping (Medium Term Scheduling):** Moving a process between main memory and a backing store. A process may be swapped out to free main memory temporarily and then swapped back in to continue execution.

**Degree of Multiprogramming:** The number of processes in memory.

**I/O-Bound Process:** A process that spends more of its time doing I/O than doing computations.

**CPU-bound Process:** A process that spends more time executing on CPU than it does performing I/O.

**Context Switch:** The switching of the CPU from one process or thread to another; requires performing a state save of the current process or thread and a state restore of the other.

**Cascading Termination:** A technique in which, when a process is ended, all of its children are ended as well.

**Interprocess Communication (IPC):** Communication between processes.

**Message Passing:** In interprocess communication, a method of sharing data in which messages are sent and received by processes. Packets of information in predefined formats are moved between processes or between computers.

**Direct Communication:** In interprocess communication, a communication mode in which each process that wants to communicate must explicitly name the recipient or sender of the communication.

**Blocking (Synchronous):** In interprocess communication, a mode of communication in which the sending process is blocked until the message is received by the receiving process or by a mailbox and the receiver blocks until a message is available. In I/O, a request that does not return until the I/O completes.

**Buffer:** A memory area that stores data being transferred (e.g., between two devices or between a device and a process).

**Shared Memory:** In interprocess communication, a section of memory shared by multiple processes and used for message passing.

**Unbounded Buffer:** A buffer with no practical limit on its memory size.

**Bounded Buffer:** A buffer with a fixed size.

**Pipe:** A logical conduit allowing two processes to communicate.

**Named Pipes:** A connection-oriented messaging mechanism (e.g., allowing processes to communicate within a single computer system).

**Socket:** An endpoint for communication. An interface for network I/O.

**Remote Procedure Calls (RPCs):** Procedure calls sent across a network to execute on another computer; commonly used in client-server computing.

**Local Procedure Call:** In Windows, a method used for communication between two processes on the same machine.

**Thread:** A process control structure that is an execution location. A process with a single thread executes only one task at a time, while a multithreaded process can execute a task per thread.

**Multithreaded:** A term describing a process or program with multiple threads of control, allowing multiple simultaneous execution points.

**Data Parallelism:** A computing method that distributes subsets of the same data across multiple cores and performs the same operation on each core.

**Task Parallelism:** A computing method that distributes tasks (threads) across multiple computing cores, with each task is performing a unique operation.

**User Thread:** A thread running in user mode.

**Kernel Threads:** Threads running in kernel mode.

**Lightweight Process (LWP):** A virtual processor-like data structure allowing a user thread to map to a kernel thread.

**Thread Library:** A programming library that provides programmers with an API for creating and managing threads.

**Hyper-Threading:** Intel’s technology for assigning multiple hardware threads to a single processing core.

**Thread Cancellation:** Termination of a thread before it has completed.  

**Implicit Threading:** A programming model that transfers the creation and management of threading from application developers to compilers and run-time libraries.

**Thread Pool:** A number of threads created at process startup and placed in a pool, where they sit and wait for work.

**CPU burst:** Scheduling process state in which the process executes on CPU.

**I/O Burst:** Scheduling process state in which the CPU performs I/O.

**Nonpreemptive:** Scheduling in which, once a core has been allocated to a thread, the thread keeps the core until it releases the core either by terminating or by switching to the waiting state.

**Preemptive:** A form of scheduling in which processes or threads are involuntarily moved from the running state (e.g., by a timer signaling the kernel to allow the next thread to run).

**Dispatcher:** The kernel routine that gives control of a core to the thread selected by the scheduler.

**Dispatch Latency:** The amount of time the dispatcher takes to stop one thread and put another thread onto the CPU.

**Throughput:** Generally, the amount of work done over time. In scheduling, the number of threads completed per unit time.

**Response Time:** The amount of time it takes the system to respond to user action.

**First-Come First-Served (FCFS):** The simplest scheduling algorithm. The thread that requests a core first is allocated the core first.

**Convoy Effect:** A scheduling phenomenon in which a number of threads wait for one thread to get off a core, causing overall device and CPU utilization to be suboptimal.

**Shortest-Job-First (SJF):** A scheduling algorithm that associates with each thread the length of the thread’s next CPU burst and schedules the shortest first.

**Shortest-Remaining-Time-First (SJRF):** A scheduling algorithm that gives priority to the thread with the shortest remaining time until completion.

**Priority Scheduling:** A scheduling algorithm in which a priority is associated with each thread and the free CPU core is allocated to the thread with the highest priority.

**Starvation:** The situation in which a process or thread waits indefinitely within a semaphore. Also, a scheduling risk in which a thread that is ready to run is never put onto the CPU due to the scheduling algorithm; it is starved for CPU time.

**Aging:** A solution to scheduling starvation that involves gradually increasing the priority of threads as they wait for CPU time.

**Round-Robin (RR):** A scheduling algorithm similar to FCFS scheduling, but with preemption added to enable the system to switch between threads; designed especially for time-sharing systems.

**Time Quantum (Time Slice):** A small unit of time used by scheduling algorithms as a basis for determining when to preempt a thread from the CPU to allow another to run.

**Multilevel Queue:** A scheduling algorithm that partitions the ready queue into several separate queues.

**Foreground:** Describes a process or thread that is interactive (has input directed to it), such as a window currently selected as active or a terminal window currently selected to receive input.

**Interactive:** Describes a type of computing that provides direct communication between the user and the system.

**Background:** Describes a process or thread that is not currently interactive (has no interactive input directed to it), such as one not currently being used by a user.

**Multilevel Feedback Queue:** A scheduling algorithm that allows a process to move between queues.

**Real-Time:** A term describing an execution environment in which tasks are guaranteed to complete within an agreed-to time.

**Real-Time Operating Systems (RTOS):** Systems used when rigid time requirements have been placed on the operation of a processor or the flow of data; often used as control devices in dedicated applications.

**Soft Real-Time Systems:** Systems that provide no guarantee as to when a critical real-time thread will be scheduled; they guarantee only that the thread will be given preference over noncritical threads.

**Hard Real-Time Systems:** Systems in which a thread must be serviced by its deadline; service after the deadline has expired is the same as no service at all.



## Process Synchronization

**Race Condition:** A situation in which two threads are concurrently trying to change the value of a variable.

**Entry Section:** The section of code within a process that requests permission to enter its critical section.

**Critical Section:** A section of code responsible for changing data that must only be executed by one thread or process at a time to avoid a race condition.

**Exit Section:** The section of code within a process that cleanly exits the critical section.

**Remainder Section:** Whatever code remains to be processed after the critical and exit sections.

**Mutual Exclusion:** A property according to which only one thread or process can be executing code at once.

**Bounded Waiting:** A practice that places limits on the time a thread or process is forced to wait for something.

**Peterson’s Solution:** A historically interesting algorithm for implementing critical sections.

**Atomic Variable:** A programming language construct that provides atomic operations on basic data types such as integers and booleans.

**Atomically:** A computer activity (such as a CPU instruction) that operates as one uninterruptable unit.

**Busy Waiting:** A practice that allows a thread or process to use CPU time continuously while waiting for something. An I/O loop in which an I/O thread continuously reads status information while waiting for I/O to complete.

**Mutex Lock:** A mutual exclusion lock; the simplest software tool for assuring mutual exclusion.

**Spinlock:** A locking mechanism that continuously uses the CPU while waiting for access to the lock.

**Semaphore:** An integer variable that, apart from initialization, is accessed only through two standard atomic operations: wait() and signal().

**Counting Semaphore:** A semaphore that has a value between 0 and N, to control access to a resource with N instances.

**Binary Semaphore:** A semaphore of values 0 and 1 that limits access to one resource (acting similarly to a mutex lock).

**Readers-Writers Problem:** A synchronization problem in which one or more processes or threads write data while others only read data.

**Dining-Philosophers Problem:** A classic synchronization problem in which multiple operators (philosophers) try to access multiple items (chopsticks) simultaneously.

**Monitor:** A high-level language synchronization construct that protects variables from race conditions.

**Condition Variable:** A component of a monitor  lock; a container of threads waiting for a condition  to be true to enter the critical section.

**Deadlock:** The state in which two processes or threads are stuck waiting for an event that can only be caused by one of the processes or threads.

**Deadlock Prevention:** A set of methods intended to ensure that at least one of the necessary conditions for deadlock cannot hold.

**Deadlock Avoidance:** An operating system method in which processes inform the operating system of which resources they will use during their lifetimes so the system can approve or deny requests to avoid deadlock.

**System Resource-Allocation Graph:** A directed graph for precise description of deadlocks.

**Claim Edge:** In the deadlock resource-allocation graph algorithm, an edge indicating that a process might claim a resource in the future.

**Request Edge:** In a system resource-allocation graph, an edge (arrow) indicating a resource request.

**Assignment Edge:** In a system resource-allocation graph, an edge (arrow) indicating a resource assignment.

**Safe State:** In deadlock avoidance, a state in which a system can allocate resources to each process in  some order and still avoid deadlock.

**Safe Sequence:** “In deadlock avoidance, a sequence of processes in which, for each Pi, the resource requests that Pi can make can be satisfied by the currently available resources plus the resources held by all Pj, with j < i.”

**Banker’s Algorithm:** A deadlock avoidance algorithm, less efficient than the resource-allocation graph scheme but able to deal with multiple instances of each resource type.

**Wait-For Graph:** In deadlock detection, a variant of the resource-allocation graph with resource nodes removed; indicates a deadlock if the graph contains a cycle.



## Memory Management

**Random-Access Memory (RAM):** Rewritable memory, also called main memory. Most programs run from RAM, which is managed by the kernel.

**Dynamic Random-Access Memory (DRAM):** The common version of RAM, which features high read and write speeds.

**Base Register:** A CPU register containing the starting address of an address space. Together with the limit register, it defines the logical address space.

**Limit Register:** A CPU register that defines the size of the range. Together with the base register, it defines the logical address space.

**Logical Address (Virtual Address):** Address generated by the CPU; must be translated to a physical address before it is used.

**Logical Address Space:** The set of all logical addresses generated by a program.

**Physical Address:** Actual location in physical memory of code or data. 

**Physical Address Space:** The set of all physical addresses generated by a program.

**Memory-Management Unit (MMU):** The hardware component of a computer CPU or motherboard that allows it to access memory.

**Relocation Register:** A CPU register whose value is added to every logical address to create a physical address (for primitive memory management).

**Bind:** Tie together. For example, a compiler binds a symbolic address to a relocatable address so the routine or variable can be found during execution.

**Relocation:** An activity associated with linking and loading that assigns final addresses to program parts and adjusts code and data in the program to match those addresses.

**Absolute Code:** Code with bindings to absolute memory addresses.

**Relocatable Code:** Code with bindings to memory addresses that are changed at loading time to reflect where the code is located in main memory.

**Relocatable Object file:** The output of a compiler in which the contents can be loaded into any location in physical memory.

**Linker:** A system service that combines relocatable object files into a single binary executable file.

**Executable File:** A file containing a program that is ready to be loaded into memory and executed.

**Loader:** A system service that loads a binary executable file into memory, where it is eligible to run on a CPU core.

**Dynamic Loading:** The loading of a process routine when it is called rather than when the process is started.

**Dynamically Linked Libraries (DLLs):** System libraries that are linked to user programs when the processes are run, with linking postponed until execution time.

**Shared Libraries:** Libraries that can be loaded into memory once and used by many processes; used in systems that support dynamic linking.

**Contiguous Memory Allocation:** A memory allocation method in which each process is contained in a single section of memory that is contiguous to the section containing the next process.

**Variable-Partition:** A simple memory-allocation scheme in which each partition of memory contains exactly one process.

**Hole:** In variable partition memory allocation, a contiguous section of unused memory.

**Dynamic Storage-Allocation Problem:** The problem of how to satisfy a request for size n of memory from a list of free holes.

**First-Fit:** In memory allocation, selecting the first hole large enough to satisfy a memory request.

**Best-Fit:** In memory allocation, selecting the smallest hole large enough to satisfy the memory request.

**Worst-Fit:** In memory allocation, selecting the largest hole available.

**Internal Fragmentation:** Fragmentation that is internal to a partition.

**External Fragmentation:** Fragmentation in which available memory contains holes that together have enough free space to satisfy a request but no single hole is large enough to satisfy the request. More generally, the fragmentation of an address space into small, less usable chunks.

**Compaction:** The shuffling of storage to consolidate used space and leave one or more large holes available for more efficient allocation.

**Paging:** A common memory management scheme that avoids external fragmentation by splitting physical memory into fixed-sized frames and logical memory into blocks of the same size called pages.

**Frames:** Fixed-sized blocks of physical memory.

**Page:** A fixed-sized block of logical memory.

**Page Number:** Part of a memory address generated by the CPU in a system using paged memory; an index into the page table.

**Page Offset:** Part of a memory address generated by the CPU in a system using paged memory; the offset of the location within the page of the word being addressed.

**Page Table:** In paged memory, a table containing the base address of each frame of physical memory, indexed by the logical page number.

**Page-Table Base Register:** In paged memory, the CPU register pointing to the in-memory page table.

**Page-Table Length Register:** A CPU register indicating the size of the page table.

**Translation Look-Aside Buffer (TLB):** A small, fast-lookup hardware cache used in paged memory address translation to provide fast access to a  subset of memory addresses.

**Address-Space Identifier:** A part of a TLB entry that identifies the process associated with that entry and, if the requesting process doesn’t match the ID, causes a TLB miss for address-space protection.

**TLB Miss:** A translation look-aside buffer lookup that fails to provide the address translation because it is not in the TLB.

**Hit Ratio:** The percentage of times a cache provides a valid lookup (used, e.g., as a measure of a TLB’s effectiveness).

**Effective Memory-Access Time:** The statistical or real measure of how long it takes the CPU to read or write to memory.

**Valid-Invalid:** A page-table bit indicating whether a page-table entry points to a page within the logical address space of that process.

**Hashed Page Table:** A page table that is hashed for faster access; the hash value is the virtual page number.

**Inverted Page Table:** A page-table scheme that has one entry for each real physical page frame in memory; the entry maps to a logical page (virtual address) value.

**Swapping:** Moving a process between main memory and a backing store. A process may be swapped out to free main memory temporarily and then swapped back in to continue execution.

**Backing Store:** The secondary storage area used for process swapping.

**Virtual Memory:** A technique that allows the execution of a process that is not completely in memory. Also, separation of computer memory address space from physical into logical, allowing easier programming and larger name space.

**Heap Section:** The section of process memory that is dynamically allocated during process run time; it stores temporary variables.

**Stack Section:** The section of process memory that contains the stack; it contains activation records and other temporary data.

**Demand Paging:** In memory management, bringing in pages from storage as needed rather than, e.g., in their entirety at process load time.

**Lazy Swapper:** A swapping method in which only pages that are requested are brought from secondary storage into main memory.

**Pager:** The operating-system component that handles paging.

**Copy-On-Write (COW):** Generally, the practice by which any write causes the data to first be copied and then modified, rather than overwritten. In virtual memory, on a write attempt to a shared page, the page is first copied, and the write is made to that copy.

**Memory-Mapped File:** A file that is loaded into physical memory via virtual memory methods, allowing access by reading and writing to the memory address occupied by the file.

**Page Fault:** A fault resulting from a reference to a non-memory-resident page of memory.

**Page-Fault Rate:** A measure of how often a page fault occurs per memory access attempt.

**Page Replacement:** In virtual memory, the selection of a frame of physical memory to be replaced when a new page is allocated.

**Over-Allocating:** Generally, providing access to more resources than are physically available. In virtual memory, allocating more virtual memory than there is physical memory to back it.

**Modify Bit:** An MMU bit used to indicate that a frame has been modified (and therefore must have its contents saved before page replacement).

**Page-Replacement Algorithm:** In memory management, the algorithm that chooses which victim frame of physical memory will be replaced by a needed new frame of data.

**Victim Frame:** In virtual memory, the frame selected by the page-replacement algorithm to be replaced.

**Belady’s Anomaly:** An anomaly in frame-allocation algorithms in which page-fault rate may increase as the number of allocated frames increases.

**Optimal Page-Replacement Algorithm:** A theoretically optimal page replacement algorithm that has the lowest page-fault rate of all algorithms and never suffers from Belady’s anomaly.

**Least Recently Used (LRU):** In general, an algorithm that selects the item that has been used least recently. In memory management, selecting the page that has not been accessed in the longest time.

**Locality:** The tendency of processes to reference memory in patterns rather than randomly.

**Least Frequently Used (LFU):** In general, an algorithm that selects the item that has been used least frequently. In virtual memory, when access counts are available, selecting the page with the lowest count.

**Most Frequently Used (MFU):** In general, an algorithm that selects the item that has been used most frequently. In virtual memory, when access counts are available, selecting the page with the highest  count.

**Global Replacement:** In virtual memory frame allocation, allowing a process to select a replacement frame from the set of all frames in the system, not just those allocated to the process.

**Local Replacement:** In virtual-memory frame allocation, allowing a process to select a replacement frame only from the set of all frames allocated to the process.

**Thrashing:** Paging memory at a high rate. A system thrashes when there is insufficient physical memory to meet virtual memory demand.

**Locality Model:** A model for page replacement based on the working-set strategy.

**Working-Set Model:** A model of memory access based on tracking the set of most recently accessed pages.

**Working-Set Window:** A limited set of most recently accessed pages (a “window” view of the entire set of accessed pages).

**Page-Fault Frequency:** The frequency of page faults.



## Storage Management

**Secondary Storage:** A storage system capable of holding large amounts of data permanently; most commonly, HDDs and NVM devices.

**Nonvolatile Memory (NVM):** Persistent storage based on circuits and electric charges.

**Hard Disk Drive (HDD):** A secondary storage device based on mechanical components, including spinning magnetic media platters and moving read-write heads.

**Transfer Rate:** The rate at which data flows.

**Seek Time:** On an HDD, the time it takes the read-write head to position over the desired cylinder.

**Rotational Latency:** On an HDD, the time it takes the read-write head, once over the desired cylinder, to access the desired track.

**Head Crash:** On an HDD, a mechanical problem involving the read-write head touching a platter.

**I/O Bus:** A physical connection of an I/O device to a computer system.

**Platter:** An HDD component that has a magnetic media layer for holding charges.

**Track:** On an HDD platter, the medium that is under the read-write head during a rotation of the platter.

**Cylinder:** On an HDD, the set of tracks under the read-write heads on all platters in the device.

**Magnetic Tape:** A magnetic media storage device consisting of magnetic tape spooled on reels and passing over a read-write head. Used mostly for backups.

**Bandwidth:** The total amount of data transferred divided by the total time between the first request for service and the completion of the last transfer.

**Shortest-Seek-Time-First (SSTF) Algorithm:** An HDD I/O scheduling algorithm that sorts requests by the amount of seek time required to accomplish the request; the shortest time has the highest priority.

**SCAN Algorithm:** An HDD I/O scheduling algorithm in which the disk head moves from one end of the disk to the other performing I/O as the head passes the desired cylinders; the head then reverses direction and repeats.

**Circular SCAN (CSCAN) Scheduling:** An HDD I/O scheduling algorithm in which the disk head moves from one end of the disk to the other performing I/O as the head passes the desired cylinders; the head then reverses direction and continues.

**LOOK:** An HDD I/O scheduling algorithm modification of SCAN that stops the head after the last request is complete (rather than at the innermost or outermost cylinder).

**CLOOK Scheduling:** An HDD I/O scheduling algorithm that modifies CSCAN by stopping the head after the final request is completed (rather than at the innermost or outermost cylinder).

**Low-Level Formatting (Physical Formatting):** The initialization of a storage medium in preparation for its use as a computer storage device.

**Partition:** Logical segregation of storage space into multiple area; e.g., on HDDs, creating several groups of contiguous cylinders from the devices’ full set of cylinders.

**Volume:** A container of storage; frequently, a device containing a mountable file system (including a file containing an image of the contents of a device).

**Logical Formatting:** The creation of a file system in a volume to ready it for use.

**Sector Sparing:** The replacement of an unusable HDD sector (bad block) with another sector at some other location on the device.

**Redundant Arrays of Independent Disks (RAID):** A disk organization technique in which two or more storage devices work together, usually with protection from device failure.

**Solid-State Disk (SSD):** A disk-drive-like storage device that uses flash-memory-based nonvolatile memory.



## File System

**File:** The smallest logical storage unit; a collection of related information defined by its creator.

**File Attributes:** Metadata about a file, such as who created it and when, file size, etc.

**File System:** The system used to control data storage and retrieval; provides efficient and convenient access to storage devices by allowing data to be stored, located, and retrieved easily.

**Sequential Access:** A file-access method in which contents are read in order, from beginning to end.

**Direct Access (Relative Access):** A file-access method in which contents are read in random order, or at least not sequentially.

**Relative Block Number:** An index relative to the beginning of a file. The first relative block of the file is block 0, the next is block 1, and so on through the end of the file.

**Path Name:** The file-system name for a file, which contains all the mount-point and directory-entry information needed to locate the file (e.g., “C:/ foo/bar.txt” and “/foo/bar.txt”).

**Absolute Path Name:** A path name starting at the top of the file system hierarchy.

**Relative Path Name:** A path name starting at a relative location (such as the current directory).

**Mounting:** Making a file system available for use by logically attaching it to the root file system.

**Mount Point:** The location within the file structure where a file system is attached.

**Network File system (NFS):** A common network file system used by UNIX, Linux, and other operating systems to share files across a network.

**User Identifier (user ID) (UID):** A unique numerical user identifier.

**Group Identifier:** Similar to a user identifier, but used to identify a group of users to determine access rights.

**File-control block (FCB):** A per-file block that contains all the metadata about a file, such as its access permissions, access dates, and block locations.

**Metadata:** A set of attributes of an object. In file systems, e.g., all details of a file except the file’s contents.

**Logical File system:** A logical layer of the operating system responsible for file and file-system metadata management; maintains the FCBs.

**File-Organization Module:** A logical layer of the operating system responsible for files and for translation of logical blocks to physical blocks.

**Basic File System:** A logical layer of the operating system responsible for issuing generic commands to the I/O control layer, such as “read block x,” and also buffering and caching I/O.

**I/O Control:** A logical layer of the operating system responsible for controlling I/O, consisting of device drivers and interrupt handlers.

**Virtual File System (VFS):** The file-system implementation layer responsible for separating file-system-generic operations and their implementation and representing a file throughout a network. 

**Contiguous Allocation:** A file-block allocation method in which all blocks of the file are allocated as a contiguous chunk on secondary storage.

**Compaction:** The shuffling of storage to consolidate used space and leave one or more large holes available for more efficient allocation.

**Extent:** A chunk of contiguous storage space added to previously allocated space; a pointer is used to link the spaces.

**Linked Allocation:** A type of file-system block allocation in which each file is a linked list of allocated blocks containing the file’s contents. Blocks may be scattered all over the storage device.

**Cluster:** In Windows storage, a power-of-2 number of disk sectors collected for I/O optimization.

**File-Allocation Table (FAT):** A simple but effective method of disk-space allocation used by MS-DOS. A section of storage at the beginning of each volume is set aside to contain the table, which has one entry per block, is indexed by block number, and contains the number of the next block in the file.

**Indexed Allocation:** In file-system block allocation, combining all block pointers in one or more index blocks to address limits in linked allocation and allow direct access to each block.

**Index Block:** In indexed allocation, a block that contains pointers to the blocks containing the file’s data.

**UNIX File System (UFS):** An early UNIX file systems; uses inodes for FCB.

**Inode:** In many file systems, a per-file data structure holding most of the metadata of the file. The FCB in most UNIX file systems.

**Superblock:** The UFS volume control block.

**Direct Blocks:** In UFS, blocks containing pointers to data blocks.

**(Single) Indirect Block:** In UFS, a block containing pointers to direct blocks, which point to data blocks. 

**Double Indirect Block:** In UFS, a block containing pointers to a single indirect block, which points to data blocks.

**Triple Indirect Block:** In UFS, a block containing pointers to double indirect blocks, which point to single indirect blocks, which point to data blocks.

**Bitmap:** A string of n binary digits that can be used to represent the status of n items. The availability of each item is indicated by the value of a binary digit: 0 means that the resource is available, while 1 indicates that it is unavailable (or vice-versa).

**Free-Space List:** In file-system block allocation, the data structure tracking all free blocks in the file system.

**Consistency Checker:** A system program, such as UNIX fsck, that reads file system metadata and tries to correct any inconsistencies (such as a file’s length being incorrectly recorded).

**Log File:** A file containing error or “logging” information; used for debugging or understanding the state or activities of the system.
