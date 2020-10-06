# Translation Caching: Skip, Don’t Walk (the Page Table)

T. W. Barr, A. L. Cox, and S. Rixner, ISCA 2010

## Summary

This paper explores the design space of MMU caches that accelerate virtual-to-physical address translation in processor architectures that use a radix tree page table and shows that the most effective MMU caches are translation caches, which store partial translations and allow the page walk hardware to skip one or more levels of the page table.

## Motivations

As the physical and virtual address spaces supported by x86 processors have grown in size, the maximum depth of the radix tree page table has increased, more memory references to "walk" the page table from top to bottom to translate a virtual address are needed. In recent years, both AMD and Intel processors have implemented MMU caches. However, their implementations are quite different and represent distinct points in the design space.

## Solution

This paper explores the design space of MMU caches that accelerate virtual-to-physical address translation in processor architectures that use a radix tree page table.

### Design Space

The design space can be described in a two-dimensional table:

|                   | Unified | Split | Path |
| ----------------- | ------- | ----- | ---- |
| Page Table Cache  | UPTC    | SPTC  | N/A  |
| Translation Cache | UTC     | STC   | TPC  |

__Page table caches__ store elements from the page table tagged by their physical address in memory, as conventional data caches might. __Translation caches__ are indexed by parts of the virtual address.

For either of these tagging schemes, elements from different levels of the page table can be mixed in a single cache (a __unified__ cache), or placed into separate caches (a __split__ cache). Finally, each cache entry can store an entry from one level along the page walk, or it can store an entire path (a __path__ cache).

In particular, the unified page table cache (UPTC) is the design that appears in modern AMD x86-64 processors. The split translation cache (STC) is the design that appears in modern Intel x86-64 processors.

### Comparison

1. Indexing:

    Page table caches use the physical addresses of the page table entries as indices. Translation caches use components of the virtual address as indices.

2. Partitioning:

    The partitioning of the cache determines how the entries of the cache are allocated to different levels of the page table. The impact of the partitioning scheme largely depends on whether the application densely or sparsely utilizes its virtual address space. The unified cache needs careful design of the replacement policy for entries of each level. The split cache needs careful design of the allocation of entries to each level. The path translation cache avoids these partitioning problems.

3. Coverage:

    In general, with the same number of entries, translation caches are able to cover a larger portion of the address space than page table caches.

4. Complexity:

    For current architectural parameters, translation caches require significantly smaller tags.

### Modified LRU replacement algorithm for the unified cache

In particular, this paper introduces a new modified LRU replacement policy for the unified cache, where entries from lower levels of the page table are inserted into the LRU queue at a recency position behind the most-recently-used position. If these lower level entries are reused, they are promoted to the most recently used position. However, if they are not reused, the portion of the cache in which lower level entries compete with upper level entries is small.

## Evaluation

The MMU cache architectures were evaluated by running application memory traces through a memory system simulator with applications of different memory-access patterns. Further, they compared the memory access behavior of the cached page table with its biggest rivals, hash-table based Inverted Page Tables and direct-mapped Translation Storage Buffers.

Qualitative results show that the unified translation cache with a modified LRU replacement scheme, which is introduced in this paper, is superior to both AMD's and Intel's existing MMU cache designs. It adapts well to varying workloads (unlike Intel's ) and prevents conflict between entries of low and high reuse (unlike AMD's).

Further, it is shown that with a well designed MMU cache, the radix tree organization is superior to either hash-table based inverted page tables or direct-mapped translation storage
buffers.
