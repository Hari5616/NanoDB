# NanoDB

A storage engine I'm writing in raw C++. No STL. No iostream. I handle every byte myself.

I'm a CS undergrad and I got tired of using databases without knowing what's actually happening inside them. So I'm building one. Not to ship it — to understand it.

---

## The rules I set for myself

- No STL containers. No `std::vector`, no `std::string`, nothing.
- No `iostream`. All I/O goes through POSIX or `<cstdio>`.
- Raw pointers and structs only.
- If I don't understand why something works, I don't use it.

---

## What I'm building

Eight phases, each one sitting on the previous.

```
Phase 7 — Concurrency (2PL lock manager)
Phase 6 — Write-Ahead Log (crash recovery, REDO log)
Phase 5 — Query Layer (KV API, Volcano iterator)
Phase 4 — B+ Tree (disk-aware, split/merge from scratch)
Phase 3 — Heap File & Tuples (slot directory, row ops)
Phase 2 — Buffer Pool (LRU eviction, dirty tracking)
Phase 1 — Page Manager (fixed pages, raw byte I/O)
Phase 0 — C++ without the safety nets
```

---

## Phase breakdown

**Phase 0 — C++ Fundamentals**  
Pointer arithmetic, raw byte arrays, POSIX I/O. No shortcuts.

**Phase 1 — Page Manager** ← where I am now  
Every database is a paged file underneath. Fixed-size pages, read and written as raw bytes. Page ID to disk offset.

**Phase 2 — Buffer Pool**  
Pages live on disk. Reads are slow. Buffer pool keeps hot pages in memory, evicts cold ones via LRU, tracks dirty pages for write-back.

**Phase 3 — Heap File & Tuple Layout**  
Rows are bytes. Slot directory for in-page free space. Insert, delete, scan at the page level.

**Phase 4 — B+ Tree Index**  
Disk-aware nodes, split and merge from scratch, range scans.

**Phase 5 — Query Layer**  
KV API over the storage layer. Table scans, point lookups, Volcano-style iterator.

**Phase 6 — Write-Ahead Log**  
REDO-only log written to disk before anything else. Checkpointing. Crash recovery.

**Phase 7 — Concurrency**  
Central lock manager, Two-Phase Locking, full transaction lifecycle.

**Phase 8 — Benchmarks & writeup**  
Valgrind clean. Benchmarked against LevelDB. Full design doc.

---

## Status

| Phase | What | Status |
|-------|------|--------|
| 0 | C++ fundamentals | in progress |
| 1 | Page manager | planned |
| 2–8 | Everything else | planned |

---

## References

- [CMU BusTub](https://github.com/cmu-db/bustub)
- [LevelDB](https://github.com/google/leveldb)
- [Architecture of a Database System](https://dsf.berkeley.edu/papers/fntdb07-architecture.pdf) — Hellerstein et al.

---

**Hariprasad Ramesh** · IIIT Sri City, CSE  
If you've built something similar, I'd like to hear from you.
