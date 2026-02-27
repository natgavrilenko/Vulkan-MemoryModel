### Test Results Summary

This document provides a summary of expected results for the sloc test suite.
This suite evaluates outcomes for basic memory patterns where a kernel accesses the same memory location through different references.

#### 1. Test Patterns
The suite evaluates three primary memory access patterns:
- **`WR`**: Write-to-Read pairs
- **`RW`**: Read-to-Write pairs
- **`WW`**: Write-to-Write pairs

#### 2. Memory Semantics
Each pattern is tested against three types of semantics:
- **`PR`**: private memory accesses
- **`NP`**: non-private memory accesses
- **`AT`**: atomic memory accesses with sufficient scope

#### 3. Ordering Relations
The test suite covers five types of relations that order memory accesses:
- **`po`**: program order
- **`ithb`**: inter-thread-happens-before
- **`po+ithb`**: program order and inter-thread-happens-before
- **`ssw`**: system-synchronized-with
- **`ssw+avvisdevice`**: system-synchronized-with combined with avdevice and visdevice operations

#### 4. Result Codes
The results are represented by specific symbols indicating observability:
- **`-`**: Not observable.
- **`R`**: Observable, **racy** behavior.
- **`S`**: Observable, **safe** behavior (no race).

#### 5. Final values
The characters in the results table represent specific observation states:

#### WW Pattern (4 characters)
1. **1st Position**: Writes store different values (no reader present).
2. **2nd Position**: The reader observes the **initial value**.
3. **3rd Position**: The reader observes the **value stored by the first write**.
4. **4th Position**: The reader observes the **value stored by the second write**.

#### WR and RW Patterns (2 characters)
1. **1st Position**: The read observes the **initial value**.
2. **2nd Position**: The read observes the **value stored by the write**.

#### 6. Summary Table

| Ordering | WW (PR \| NP \| AT) | WR (PR \| NP \| AT) | RW (PR \| NP \| AT) |
| :--- | :--- | :--- | :--- |
| **`po`** | `R-RR` \| `R-RR` \| `R-RR` | `RR` \| `RR` \| `RR` | `RR` \| `RR` \| `RR` |
| **`ithb`** | `R-RR` \| `R-RR` \| `R-RR` | `RR` \| `RR` \| `RR` | `RR` \| `S-` \| `S-` |
| **`po+ithb`** | `R-RR` \| `R-RR` \| `R-RR` | `RR` \| `RR` \| `RR` | `RR` \| `S-` \| `S-` |
| **`ssw`** | `R-RR` \| `R-RR` \| `R-RR` | `RR` \| `RR` \| `RR` | `S-` \| `S-` \| `S-` |
| **`ssw+avvisdevice`** | `S--S` \| `S--S` \| `S--S` | `-S` \| `-S` \| `-S` | `S-` \| `S-` \| `S-` |