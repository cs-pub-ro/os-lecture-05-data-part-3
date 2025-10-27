# Data (part 3)

# 0. Pitch

- **demo**: number of page faults by an application & system-level
- **demo**: resident memory used by 100 processes started from:
  - the same dynamic executable
  - the same static executable
  - 100 different dynamic executables
  - 100 different static executables
- **demo**: run application with TLB and without TLB (if possible)

# 1. Process Virtual Address Space: Perspectives

- list of zones vs array of page descriptions (PTE - page table entry)
- **diagram**: zones, page table, correspondence
- zones: managed by OS
- page table: managed by hardware / MMU / MTU

# 2. (Virtual) Memory Operations

- virtual memory reservation: only affects the list of zones view
  - reserved explicitly by a memory allocation call (`malloc()`, `mmap()`)
- **diagram**: reserve virtual memory
- allocate physical memory: only affects the page table - validity bit is updated
- allocate at access - not explicit

# 3. Page Fault

- **demo**: reservation and allocation, see `vsz`, `rss`, `min_flt`, `maj_flt` metrics
- a page fault is triggered when access is done to a page that's invalid or with the wrong permissions
- flow is: memory access instruction, aim to translate virtual address to physical address (MMU / MTU); if page table entry is invalid, trigger page fault; run page fault handler; check the zones view; allocate physical page and update page table entry
- **diagram** with flow
- run kernel space code: syscall or memory access causing a page fault
- difference between segfault or not
- **demo** ^
- **diagram**: demand paging
- **diagram**: swapping

# 4. Memory Sharing

- **diagram**: same physical memory, multiple address spaces accessing it
- implicit: executables, libraries
- explicit: `shm...` calls, require synchronization - part of App Interaction lectures

# 5. Storing the Page Table

- page table in memory
- two issues:
  - double memory access: use TLB - a cache of the page table: **diagram**
  - a lot of space use multi-level paging: **diagram**

# Conclusion and Takeaways

- virtual memory gives the appearance of using more memory than actually being used
- physical memory is allocated at access
- page table stores state of the physical page
- invalid page may mean not part of the VAS (segfault at access), or not yet allocated / mapped (no segfault at access)
- allocation is triggered by access, and by signal sent by the MMU / MTU to the operating system (page fault handler)
- frames not used are swapped on disk (swap space)
- benefit of virtual memory is memory sharing
- downside of virtual memory is space allocated for page table (alleviated with multi-level paging) and double memory access (page table + actual access) (alleviated by TLB)
