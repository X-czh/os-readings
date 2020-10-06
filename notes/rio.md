# The Rio File Cache: Surviving Operating System Crashes

P. M. Chen, W. T. Ng, S. Chandra, C. Aycock, G. Rajamani, and D. Low, ASPLOS 1996

This paper presents Rio (RAM I/O) file cache, whose goal is to make ordinary main memory safe for persistent storage by enabling main memory to survive operating system crashes.

## Motivations

1. Memories are much faster than disks.
2. Memories are conceived as unreliable.

The above points force a trade-off between performance and reliability. However, the authors argue that memory can have high reliability as well.

## Solution

This paper presents Rio (RAM I/O) file cache, which makes make ordinary main memory safe for persistent storage.

__Assumption__: the problem of power outage can be solved by uninterruptible power supply or Flash RAM.

__The Crux__: how to make RAM file content survive OS crashes.

### Protection

__Q__: How to ensure that the system does not accidentally overwrite the file cache while it is crashing?

__Solution__: Forbid non-file-cache procedures to access the file cache. One can utilize the VM protection mechanism, but many systems allow certain kernel accesses to bypass it. This paper overcomes this issue by disabling the ability of the processor to bypass the TLB.

### Warm Start

__Q__: When the system is rebooted, how to make it read the file cache contents that were present in physical memory before the crash and update the file system with this data?

__Solution__: Two parts, how to find the files and when to do the restoration. To allow the system to find the data after rebooting, the paper proposes to keep and protect a separate area of memory that contains all information needed to find, identify, and restore files in memory. Rio file cache dumps all of physical memory to the swap partition before the VM and the file system are initialized, it also restore the metadata to disk during this step. After the system is completely booted, a user-level process analyzes the memory dump and restores the data.

## Evaluation

Even without protection, Rio stores files nearly as reliably as a write-through file system. Rio with protection provides reliability even higher than a write-through file cache while issuing no reliability-induced writes to disk. On the same time, Rio's performance improvement is largest over systems that provide similar reliability guarantee.

## Concerns

1. The assumption that the problem of power outage can be easily solved is doubtful. It is unclear to me how uninterruptible power supply can scale to data centers. Also, Flash-memories tend to be much slower than memories. But it seems to be promising with recent advancements in flash memories.

2. The memory space is limited. The data eventually needs to be written to the disk. However, the performance evaluation does not mention this.
