# Library Book Arrangement using Merge Sort

## Problem Statement

A library receives thousands of books with different accession numbers and needs them arranged in ascending order.

To perform this efficiently, this project uses the **Merge Sort** algorithm.

## Why Merge Sort?

Merge Sort is suitable for large amounts of data because its time complexity is consistently **O(n log n)**.

It uses the **divide-and-conquer** approach:

1. Divide the list into two halves.
2. Recursively sort both halves.
3. Merge the sorted halves.
4. The final result is an ascending ordered list.


## Files

- `library_book_arrangement_merge_sort.ipynb` — Jupyter Notebook containing the algorithm, implementation, example, and user-input program.
- `README.md` — Project documentation.

## How to Run

### Method 1: Jupyter Notebook

1. Install Jupyter if needed:

```bash
pip install notebook
```

2. Start Jupyter Notebook:

```bash
jupyter notebook
```

3. Open `library_book_arrangement_merge_sort.ipynb`.

4. Run each cell from top to bottom.



## Example

Input:

```text
105 23 78 12 56 9 101
```

Output:

```text
Before sorting: [105, 23, 78, 12, 56, 9, 101]
After sorting: [9, 12, 23, 56, 78, 101]
```

## Algorithm

```text
MERGE_SORT(arr)

1. If length of arr <= 1:
       return arr

2. Find middle of arr

3. Divide arr into:
       left half
       right half

4. left = MERGE_SORT(left)
5. right = MERGE_SORT(right)

6. Merge left and right in ascending order

7. Return merged list
```

## Complexity Analysis

| Case | Time Complexity |
|---|---|
| Best Case | O(n log n) |
| Average Case | O(n log n) |
| Worst Case | O(n log n) |

**Space Complexity:** O(n)

