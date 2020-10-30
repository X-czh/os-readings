# The Design and Implementation of a Log-Structured File System

M. Rosenblum and J. K. Ousterhout, SOSP 1991

## Summary

The paper presents a log structured file system (LFS) where all modifications to disk are written sequentially to a log-like structure, thereby speeding up both file writing and crash recovery.

## Motivation

### Assumption

Files are cached in main memory and that increasing memory sizes will make the caches more and more effective at satisfying read requests.
As a result, disk traffic will become dominated by writes.

### Problems with previous file systems

1. They spread information around the disk in a way that causes too many small accesses, e.g., separating inodes and file contents.
2. They tend to write synchronously: the application must wait for the write to complete, rather than continuing while the write is handled in the background.

### Core question

How to increase write performance?

## Solution

### Sequential writes in log

LFS writes all new information to disk in a sequential structure called the log. This approach increases write performance dramatically by eliminating almost all seeks. The sequential nature of the log also permits much faster crash recovery.

### Inode map for fast read

LFS uses an inode map to maintain the current location of each inode.  The inode map is divided into blocks that are written to the log; a fixed checkpoint region on each disk identifies the locations of all the inode map blocks. Inode maps are compact enough to keep the active portions cached in main memory so that inode map lookups rarely require disk accesses.

### Free Space Management by Segments

The disk is divided into fixed-size extents called segments. Any given segment is always written sequentially from its beginning to its end, and all live data must be copied out of a segment before the segment can be
rewritten. A segment cleaner process continually regenerates empty segments by compressing the live data from heavily fragmented segments.

### Crash Recovery

LFS performs checkpoints at periodic intervals or when FS is unmounted or system is shut down. It writes a checkpoint region to a special fixed position on disk. To allow maximum data recovery after crashes, LFS performs roll-forward by scanning through the log segments that were written after the last checkpoint.

## Evaluation

The authors demonstrate that LFS has an order of magnitude higher performance over FFS while writing, and still maintaining a comparable read rate. Crash recovery time is reduced by using roll-forward.

## Comment

The authors have an acute observation in catching the assumption that disk traffic will become dominated by writes as reads are eliminated by memory-cached files. They give a clean yet effective solution to perform writes as sequential as possible using the log structure. There are still quite a few details that need discussion, like the segment cleaning policies.
