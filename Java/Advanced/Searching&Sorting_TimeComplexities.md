Searching Algorithms:

1. Linear Search:
   Time Complexity: O(n)

2. Binary Search:
   Time Complexity: O(logn) (for sorted arrays)
   Space Complexity: O(1)
3. Jump Search:
   Time Complexity: O(n) (for sorted arrays)
   Space Complexity: O(1)

4. Interpolation Search:

Time Complexity:
�
(
log
⁡
log
⁡
�
)
O(loglogn) average case,
�
(
�
)
O(n) worst case (for sorted arrays)
Space Complexity:
�
(
1
)
O(1)
Exponential Search:

Time Complexity:
�
(
log
⁡
�
)
O(logn) (for sorted arrays)
Space Complexity:
�
(
1
)
O(1)
Hashing (HashMaps):

Average Case Time Complexity:
�
(
1
)
O(1)
Worst Case Time Complexity:
�
(
�
)
O(n)
Space Complexity:
�
(
�
)
O(n)
Sorting Algorithms:
Bubble Sort:

Time Complexity:
�
(
�
2
)
O(n
2
)
Space Complexity:
�
(
1
)
O(1)
Inefficient for large datasets.
Selection Sort:

Time Complexity:
�
(
�
2
)
O(n
2
)
Space Complexity:
�
(
1
)
O(1)
Insertion Sort:

Time Complexity:
�
(
�
2
)
O(n
2
) average and worst case
Space Complexity:
�
(
1
)
O(1)
Merge Sort:

Time Complexity:
�
(
�
log
⁡
�
)
O(nlogn)
Space Complexity:
�
(
�
)
O(n) auxiliary space
Quick Sort:

Time Complexity:
�
(
�
log
⁡
�
)
O(nlogn) average case,
�
(
�
2
)
O(n
2
) worst case
Space Complexity:
�
(
log
⁡
�
)
O(logn) auxiliary space
Heap Sort:

Time Complexity:
�
(
�
log
⁡
�
)
O(nlogn)
Space Complexity:
�
(
1
)
O(1)
Counting Sort:

Time Complexity:
�
(
�

- �
  )
  O(n+k), where
  �
  k is the range of the input.
  Space Complexity:
  �
  (
  �
- �
  )
  O(n+k)
  Radix Sort:

Time Complexity:
�
(
�
(
�

- �
  )
  )
  O(d(n+k)), where
  �
  d is the number of digits in the maximum number and
  �
  k is the radix (usually
  �
  =
  10
  k=10)
  Space Complexity:
  �
  (
  �
- �
  )
  O(n+k)
  Bucket Sort:

Time Complexity:
�
(
�

- �
  )
  O(n+k) average case, where
  �
  k is the number of buckets.
  Space Complexity:
  �
  (
  �
- �
  )
  O(n+k)
  Timsort (Python's default sorting algorithm):

Time Complexity:
�
(
�
log
⁡
�
)
O(nlogn) average case,
�
(
�
)
O(n) best case (for nearly sorted arrays)
Space Complexity:
�
(
�
)
O(n) - as it uses merge sort and insertion sort.
