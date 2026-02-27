### Test results summary

This file provides a summary table of expected results for the sloc test suite.

The results are specified using the following symbols:
- `-` - not observable
- `R` - observable, racy behavior
- `S` - observable, safe behavior (no race)

The first row in the table specifies the pattern:
- `WR` write-to-read pairs
- `RW` read-to-write pairs
- `WW` write-to-write pairs

The second row specifies the semantics of the instructions:
- `PR` private memory accesses
- `NP` non-private memory accesses
- `AT` atomic memory accesses with sufficient scope

Results are given for different relations ordering the instructions:
- `po` - program order (the instructions are in the same thread)
- `ithb` - inter-thread-happens-before (the instructions are in the different threads)
- `po+ithb` - program order and inter-thread-happens-before (the instructions are in the same thread)
- `ssw` - system-synchronized-with (the instructions are in the different threads)
- `ssw+avvisdevice` - system-synchronized-with combined with avdevice and visdevice operation (the instructions are in the different threads)

For `WR` and `RW`, there are two results values:
1) the behavior where the read observes the initial value
2) the behavior where the read observes the value stored by the write.

For `WW`, there are four results values:
1) the behavior where the writes store different values and there is no reader
2) the behavior where the reader observed the initial value
3) the behavior where the reader observed the value stored by the first write
4) the behavior where the reader observed the value stored by the second write

```
-------------------------------------------------------------------------
|                 ||      WR      ||      RW      ||         WW         |
|                 ||-----------------------------------------------------
| Ordering        || PR | NP | AT || PR | NP | AT ||  PR  |  NP  |  AT  |
-------------------------------------------------------------------------
| po              || RR | RR | RR || RR | RR | RR || R-RR | R-RR | R-RR |
| ithb            || RR | RR | RR || RR | S- | S- || R-RR | R-RR | R-RR |
| po+ithb         || RR | RR | RR || RR | S- | S- || R-RR | R-RR | R-RR |
| ssw             || RR | RR | RR || S- | S- | S- || R-RR | R-RR | R-RR |
| ssw+avvisdevice || -S | -S | -S || S- | S- | S- || S--S | S--S | S--S |
-------------------------------------------------------------------------
```