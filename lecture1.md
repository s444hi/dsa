# Lecture 1: Searching and Big-O

## Linear Search

Linear search goes through every element one at a time until it finds the target.

**Example:** Ask someone to think of a random word. Start at the first word in the dictionary and ask "is that your word?" If yes, stop. If no, move on to the next word. Continue until the word is found.

On average this takes about n/2 guesses.

```java
public static int linearSearch(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}
```

| case | complexity | why |
|---|---|---|
| best | o(1) | the target is the first element |
| worst | o(n) | it goes through every element |
| avg | o(n) | o(n/2) = o(1/2 * n) = o(n) |

## Binary Search

Binary search only works on a **sorted** array. It checks the middle element and throws away half the remaining range each time.

**Example:** Think of a random word, then pick the word in the middle of the dictionary and ask "is that your word?" If yes, stop. If no, ask whether the word comes before the middle word. If it does, keep the left half, otherwise keep the right half. Continue until the word is found.

```java
public static int binarySearch(int[] arr, int target) {
    int low = 0;
    int high = arr.length - 1;

    while (low <= high) {
        int current = (low + high) / 2;
        if (arr[current] == target) {
            return current;
        } else if (arr[current] > target) {
            high = current - 1;
        } else {
            low = current + 1;
        }
    }
    return -1;
}
```

`low` and `high` are the two ends of the range still worth searching, inclusive. The loop keeps running while `low <= high`, because once `low` passes `high` the range is empty and the target isn't there. Both branches use `current - 1` and `current + 1` so that the index just checked gets excluded, which is what guarantees the range shrinks every loop.

| case | complexity | why |
|---|---|---|
| best | o(1) | the target is the middle element, found on the first check |
| worst | o(log n) | the range halves each loop until it's empty |
| avg | o(log n) | you still halve every time, just stopping a step or two early on average |

In general, binary search is much faster than linear search on large datasets. The tradeoff is that the array has to be sorted first, and sorting costs o(n log n).

## Big-O

To find the Big-O of an expression, keep the dominant term, drop its constant coefficient, and ignore the lower-order terms.

**Example:** 5n^2 + 3n + 20 --> o(n^2)

**Ordering from fastest to slowest:**

o(1) < o(log n) < o(n) < o(n log n) < o(n^2) < o(n^3) < o(2^n)

### Combining complexities

For **sequential** operations, you **add**, then keep the fastest-growing term.

Example: two sequential for loops = o(n) + o(n) = o(2n) = o(n)

For **nested** operations, you **multiply**.

Example: three nested for loops = o(n) * o(n) * o(n) = o(n^3)

### When two algorithms are in the same class

If two algorithms differ only by a scalar, choose the one with the smaller constant factor.

- A: 5n^2 = o(n^2)
- B: 100n^2 = o(n^2)

Both are o(n^2), so A is the better pick in practice even though Big-O can't tell them apart.
