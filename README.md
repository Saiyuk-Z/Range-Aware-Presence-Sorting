Range-Aware Presence Sorting

A range-based integer sorting approach that first determines the minimum and maximum values, then uses a direct presence/count table to reconstruct the values in ascending order.

Status: Experimental / research project.

 The underlying range-indexing idea is closely related to established direct-address and counting-sort techniques. This repository focuses on documenting, testing, and benchmarking this particular implementation rather than claiming a new sorting paradigm.

Core idea

Given an integer array A:
Scan once to find the minimum and maximum values.
Define the value range R = max(A) - min(A) + 1.
Allocate a table indexed by value - min(A).
Record how many times each value occurs.
Walk the table from low to high and reconstruct the sorted array.
No comparison-based sorting function is used.

Example

Input:
7 6 9 4 2 5 8 3 5 7 7
Output:
2 3 4 5 5 6 7 7 7 8 9

Pseudocode

RANGE_AWARE_SORT(A):
    if A is empty:
        return []

    low  = A[0]
    high = A[0]

    for x in A:
        if x < low:
            low = x
        if x > high:
            high = x

    count = array of R zeros
    where R = high - low + 1

    for x in A:
        count[x - low] += 1

    result = empty array

    for i from 0 to R - 1:
        repeat count[i] times:
            append i + low to result

    return result

Complexity

Let:
n = number of input elements
R = max(A) - min(A) + 1 = numerical range

Time:

O(n + R)

Space:

O(R)

The method is linear in n when R = O(n). It is not efficient when the numerical range is extremely large relative to n.

Important limitation

For an input such as:
[1, 1000000000]
R is approximately one billion. A direct range table is therefore impractical.
This is a range-sensitive algorithm, not an unconditional O(n) sorting algorithm.

Duplicates

The implementation uses a frequency table rather than a Boolean presence table, so duplicate values are preserved.
Comparison with established algorithms

Algorithm

Typical time
Extra space
Range-sensitive

This implementation

O(n + R)
O(R)
Yes

Counting sort
O(n + R)
O(R)
Yes

Merge sort
O(n log n)
O(n)
No

Heap sort
O(n log n)
O(1)
No

Quicksort
O(n log n) average
O(log n) typical stack
No

The most important research question is therefore not whether the asymptotic bound is new, but whether this particular implementation has a measurable practical advantage for a clearly defined workload.

Benchmark methodology

Benchmarks should:
Use the same input arrays for every algorithm.
Test multiple n values.
Test narrow, medium, and wide value ranges.
Include random, already ordered, reverse-ordered, and duplicate-heavy inputs.
Separate generation time from sorting time.
Use repeated runs and report median or minimum times.
Run on the same machine and Python version.
Verify that every implementation produces exactly the same sorted result.
The included benchmark script measures the sorting operation only and checks correctness.

Project structure

range-aware-sorting/
├── README.md
├── LICENSE
├── .gitignore
├── src/
│   └── range_aware_sort.py
├── tests/
│   └── test_range_aware_sort.py
├── benchmarks/
│   └── benchmark.py
└── docs/
    └── algorithm.md
Running

python src/range_aware_sort.py
Run tests:
python -m unittest discover -s tests
Run benchmarks:
python benchmarks/benchmark.py

Research direction

A useful next step is to benchmark this implementation against Python's built-in Timsort and a carefully implemented counting sort across controlled ranges.
Performance claims should be based on measured results rather than a single example.
