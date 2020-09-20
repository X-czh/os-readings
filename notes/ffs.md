# A Fast File System for UNIX

M. K. McKusick, W. N. Joy, S. J. Leffler, and R. S. Fabry, ACM Transactions on Computer Systems 1984

## Summary

This paper presents a fast file system for UNIX that provides substantially higher throughput rates by using more flexible allocation policies that allow better locality of reference and can be adapted to a wide range of peripheral and processor characteristics. Long-needed enhancements to the programmers' interface is also introduced.

## Motivations

The original UNIX file system is incapable of providing the data throughput rates that many applications require on new machines like VAX-11. It provides only about 2 percent of the maximum disk bandwidth of VAX-11.

Since the UNIX file system interface is well understood and not inherently slow, it is desired to retain the abstraction and simply changed the underlying implementation for higher throughput.

## Contributions

This paper presents a new file system organization that provides substantially higher throughput rates by using more flexible allocation policies that allow better locality of reference and can be adapted to a wide range of peripheral and processor characteristics. In addition, long-needed enhancements to the programmers' interface is also introduced. Compatibility to the old programmers' interface is retained.

### New File System Organization

* Larger, variable block size:
  * The block size is increased from 512B to at least 4096B for higher disk throughput rate. The block size is also variable at FFS creation time for greater flexibility.
* Block fragments:
  * To store small files in a more efficient way to avoid waste when using large blocks, a single block can be partitioned int one or more fragments.
* Cylinder groups:
  * A disk partition is divided into one more areas called cylinder groups, each of which is comprised of one more consecutive cylinders on a disk. Since disk seeks are avoided when accessing files on the same cylinder group, this provides a good abstraction of underlying hardware for implementing disk layout policies.
* Parameterized file system:
  * FFS tries to exploit the characteristics of the system (processor speed, properties of the disk) in order to achieve greater performance.
* Layout policies with better locality of reference:
  * Two levels of layout policies (global and local) are designed. The global layout policies decide the placement of new directories and files, and tries to improve performance by clustering related information. The local layout policies use a locally optimal scheme to lay out data blocks.

### Functional Enhancement

1. Long file names
2. File locking (advisory locks)
3. Symbolic links: A symbolic link is implemented as a file that contains a pathname. It allows references across physical file systems and intermachine linkage.
4. New system call for Rename
5. Disk quotas

## Evaluation

Empirical studies on the long term performance of the new file system is presented. It is shown that file access rates of up to ten times faster than the traditional UNIX file system are experienced. However, as is shown in Table II, much higher CPU utilization is observed when using the new FFS, this might be a problem.

## Takeaways

I like the concept of cylinder groups, it is a nice abstraction of the underlying hardware that eases the development of data layout policies. Similarly is the parameterization of file system. This is increasingly important to deal with emerging heterogeneous storage devices today.
