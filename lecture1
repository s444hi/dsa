# lecture 1
linear search: goes through every element
  ex. ask the user to think of a rand word, start at the first word in dictionary. is that your word? 
  if yes stop, else move on to the next word. cont until word found
on avg this takes n/2 guesses

public static int linearSearch(int[] arr, int target) {
        // your code here
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i;
            }
        }
        return -1;
    }

best case: o(1) bcs it's the first element
worst case: o(n) bcs it goes through every element
avg case: o(n/2) = o(1/2 * n) = o(n)

binary search: 
  ex. think of a random word, pick the word in the middle of the dictionary. is that your word?
  if yes stop, else does the word come before the middle word?
    if yes, keep the left half, else keep the right half. cont until found

public static int binarySearch(int[] arr, int target) {
        // your code here
        int low = 0;
        int high = arr.length - 1;
        int current = (low + high) / 2;

        while (low <= high) {
            if (arr[current] == target) {
                return current;
            } else if (arr[current] > target) {
                high = current - 1;
                current = (low + high) / 2;
            } else {
                low = current + 1;
                current = (low + high) / 2;
            }
        }
        return -1;
    }

best case: o(1) bcs the target is the middle element, found on the first check
worst case: o(log n) bcs the range halves each loop until it's empty
avg case: o(log n) bcs you still half every time, just stopping a step or two early on avg

in general, binary search is much faster than linear for large datasets



big o:
keep the dominant term, drop its constant coeff, ignore lower order
ex. 5n^2 + 3n + 20 --> o(n^2)

o(1) < o(log n) < o(n) < o(n log n) < o(n^2) < o(n^3) < o(2^n)

for sequential operations, you add
ex. two seq for loops = o(n) + o(n) = o(2n) = o(n)
  sequential parts --> ADD their complexities --> KEEP the fastest growing term

for nested segments, you multiply
ex. 3 nested for loops = o(n) * o(n) * o(n) = o(n^3)

if it's a situation where the algorithms only differ by a scalar:
  A: 5n^2 = o(n^2)
  B: 100n^2 = o(n^2)
choose the algorithm with the smallest constant factor
