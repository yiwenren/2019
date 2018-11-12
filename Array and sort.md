# Selection sort
In each round find the min of remaining numbers and put it in left side of array.
[-1, -10, 5, 2 3]
* iteration 1 : [-10 | -1, 5, 2, 3]
* iteration 2 : [-10, -1 | 5, 2, 3]
* iteration 3 : [-10, -1, 2 | 5, 3]
* iteration 4 : [-10, -1, 2, 3 | 5]

time complexity: O(n^2) => 
```java
for (int i = 0; i < n - 1; i++) {
   for (int j = i + 1; j < n; j++)
}
```
1 + 2 + 3 + ... + n(n - 1)/2 => n^2 
```java
public void sortIntegers(int[] A) {
     // selection sort
     int length = A.length;
     for (int i = 0; i < length - 1; i++) {
         int min = i;
         for (int j = i + 1; j < length; j++) {
             if (A[j] < A[min]) {
                 min = j;
             }
         }
         int temp = A[min];
         A[min] = A[i];
         A[i] = temp;
     }
 }
```
