# Chapter 2. Sorting

## Bubble Sort

두 인접한 원소를 검사하여 정렬하는 방법으로 시간 복합도가 O\(n^2\)로 상당히 느리지만, 코드가 단순하기 때문에 자주 사용된다. 원소의 이동이 거품이 수면으로 올라오는 듯한 모습을 보이기 때문에 지어진 이름이다. 버블소트는 쉽게 풀이해서 두개의 원소를 비교하여 자리를 교환한다. if \(items\[n\] &gt; items \[n+1\]\) 즉, 앞 뒤 수를 비교하여 앞 수가 크다면 조건이 성립되면 데이터를 서로 교체한다.

자바에선 편리하게도 배열, 리스트 형태의 데이터를 정렬해주는 클래스가 존재한다. Arrays 클래스의 sort 메서드를 통해 byte, int, short 배열 등 각 타입배열에 따른 정렬을 제공해주고, Collections 클래스의 sort, reverse 메서드를 통해 List 타입의 자료를 정렬해준다. 두 방식 모두 순차방식 으로 작동한다. 자바 1.8 부터 Arrays 클래스에는 좀더 향상된 병렬처리로 parallelSort 메서드를 제공한다.

```java
package com.example.sorting;

import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class BubleSort {

    private static void sortDesc(int[] numbers) {
        System.out.printf("\t%s\n", Arrays.toString(numbers));
        for (int i = 0; i < numbers.length - 1; i++) {
            for (int j = 0; j < numbers.length - 1; j++) {
                // 뒤 아이템이 앞보다 큰 경우 위치를 바꾼다.
                if (numbers[j] < numbers[j + 1]) {
                    int temp = numbers[j];
                    numbers[j] = numbers[j + 1];
                    numbers[j + 1] = temp;
                    System.out.printf("\t%s\n", Arrays.toString(numbers));
                }
            }
            System.out.printf("\t----cycle %d----\n", i + 1);
        }
    }

    private static void sortAsc(int[] numbers) {
        System.out.printf("\t%s\n", Arrays.toString(numbers));
        for (int i = 0; i < numbers.length - 1; i++) {
            for (int j = 0; j < numbers.length - 1; j++) {
                // 앞 아이템이 뒤 아이템보다 큰 경우 위치를 바꾼다.
                if (numbers[j] > numbers[j + 1]) {
                    int temp = numbers[j];
                    numbers[j] = numbers[j + 1];
                    numbers[j + 1] = temp;
                    System.out.printf("\t%s\n", Arrays.toString(numbers));
                }
            }
            System.out.printf("\t----cycle %d----\n", i + 1);
        }
    }

    public static void main(String[] args) {
        int[] numbers = { 5, 4, 3, 2, 1 };

        // 오름차순 정렬
        sortAsc(numbers);
        System.out.println("결과 " + Arrays.toString(numbers) + "\n");

        // 내림차순 정렬
        sortDesc(numbers);
        System.out.println("결과 " + Arrays.toString(numbers) + "\n");

        System.out.println("========================\n");

        Integer[] newArray = new Integer[numbers.length];
        for (int i = 0; i < newArray.length; i++) {
            newArray[i] = Integer.valueOf(numbers[i]);
        }
        // 배열을 리스트로 변환한다. 이 경우 리스트에 아이템 추가는 불가능하다.
        List<Integer> list = Arrays.asList(newArray);

        // 컬렉션 오름차순 정렬
        Collections.sort(list);
        System.out.println("결과 " + list + "\n");

        // 컬렉션 내림차순 정렬
        Collections.reverse(list);
        System.out.println("결과 " + list + "\n");

        // 원시타입 오름차순 정렬
        Arrays.sort(numbers);
        System.out.println("결과 " + Arrays.toString(numbers) + "\n");

        // 객체타입 내림차순 정렬
        Arrays.sort(newArray, Comparator.reverseOrder());
        System.out.println("결과 " + Arrays.toString(newArray) + "\n");
    }
}
```

```
    [5, 4, 3, 2, 1]
    [4, 5, 3, 2, 1]
    [4, 3, 5, 2, 1]
    [4, 3, 2, 5, 1]
    [4, 3, 2, 1, 5]
    ----cycle 1----
    [3, 4, 2, 1, 5]
    [3, 2, 4, 1, 5]
    [3, 2, 1, 4, 5]
    ----cycle 2----
    [2, 3, 1, 4, 5]
    [2, 1, 3, 4, 5]
    ----cycle 3----
    [1, 2, 3, 4, 5]
    ----cycle 4----
결과 [1, 2, 3, 4, 5]

    [1, 2, 3, 4, 5]
    [2, 1, 3, 4, 5]
    [2, 3, 1, 4, 5]
    [2, 3, 4, 1, 5]
    [2, 3, 4, 5, 1]
    ----cycle 1----
    [3, 2, 4, 5, 1]
    [3, 4, 2, 5, 1]
    [3, 4, 5, 2, 1]
    ----cycle 2----
    [4, 3, 5, 2, 1]
    [4, 5, 3, 2, 1]
    ----cycle 3----
    [5, 4, 3, 2, 1]
    ----cycle 4----
결과 [5, 4, 3, 2, 1]

========================

결과 [1, 2, 3, 4, 5]

결과 [5, 4, 3, 2, 1]

결과 [1, 2, 3, 4, 5]

결과 [5, 4, 3, 2, 1]
```

## Insertion Sort

```java
package com.example.sorting;

import java.util.Arrays;

public class MyInsertionSort {
    private static final boolean LOG_ON = true;

    public void sort(int[] numbers) {
        int temp, j;

        for (int i = 0; i < numbers.length; i++) {

            temp = numbers[i];
            if (LOG_ON) System.out.println("\t시작 temp = " + temp);
            for (j = i; j > 0 && numbers[j - 1] > temp; j--) {
                // 앞에 값이 기준 값(temp)보다 큰 경우 앞에 값을 뒤로 옮긴다.
                // temp가 보유한 값을 넣을 자리를 계산하기 위해서 j-- 한다.
                numbers[j] = numbers[j - 1];
                if (LOG_ON) System.out.println("\t교환 " + Arrays.toString(numbers));
            }
            // temp가 보유한 값을 계산된 위치에 삽입한다.
            numbers[j] = temp;

            if (LOG_ON) System.out.println("\t완료 " + Arrays.toString(numbers));
            if (LOG_ON) System.out.printf("\t------------ %d cycle ------------ \n", (i + 1));
        }
    }

    public static void main(String[] args) {
        int[] numbers = { 5, 4, 3, 2, 1 };
        System.out.println("Original");
        System.out.println(Arrays.toString(numbers) + "\n");

        MyInsertionSort sorter = new MyInsertionSort();

        sorter.sort(numbers);
        System.out.println("Asc");
        System.out.println(Arrays.toString(numbers) + "\n");
    }

}
```

```
Original
[5, 4, 3, 2, 1]

    시작 temp = 5
    완료 [5, 4, 3, 2, 1]
    ------------ 1 cycle ------------ 
    시작 temp = 4
    교환 [5, 5, 3, 2, 1]
    완료 [4, 5, 3, 2, 1]
    ------------ 2 cycle ------------ 
    시작 temp = 3
    교환 [4, 5, 5, 2, 1]
    교환 [4, 4, 5, 2, 1]
    완료 [3, 4, 5, 2, 1]
    ------------ 3 cycle ------------ 
    시작 temp = 2
    교환 [3, 4, 5, 5, 1]
    교환 [3, 4, 4, 5, 1]
    교환 [3, 3, 4, 5, 1]
    완료 [2, 3, 4, 5, 1]
    ------------ 4 cycle ------------ 
    시작 temp = 1
    교환 [2, 3, 4, 5, 5]
    교환 [2, 3, 4, 4, 5]
    교환 [2, 3, 3, 4, 5]
    교환 [2, 2, 3, 4, 5]
    완료 [1, 2, 3, 4, 5]
    ------------ 5 cycle ------------ 
Asc
[1, 2, 3, 4, 5]
```

## Selection Sort

```java
package com.example.sorting;

import java.util.Arrays;

public class MySelectionSort2 {
    private static final boolean LOG_ON = true;

    public void sort(int[] numbers, boolean... bs) {
        int min = 0;

        for (int i = 0; i < numbers.length - 1; i++) {
            if (LOG_ON) System.out.println("\t기준 i = " + i);
            min = i;
            if (LOG_ON) System.out.println("\t초기 min = " + min);

            // 가장 작은 값의 인덱스를 찾아서 변수 min에 담는다.
            for (int j = i + 1; j < numbers.length; j++) {
                if (numbers[min] > numbers[j]) {
                    min = j;
                    if (LOG_ON) System.out.println("\t교환 min = " + min);
                }
            }
            // 기준위치의 아이템을 가장 작은 아이템과 교환한다.
            int temp = numbers[i];
            numbers[i] = numbers[min];
            numbers[min] = temp;

            if (LOG_ON) System.out.println("\t" + Arrays.toString(numbers));
            if (LOG_ON) System.out.printf("\t--- %d cycle --- \n", (i + 1));
        }

    }

    public static void main(String[] args) {
        int[] numbers = { 5, 4, 3, 2, 1 };
        System.out.println("Original");
        System.out.println(Arrays.toString(numbers));
        System.out.println();

        MySelectionSort2 sorter = new MySelectionSort2();

        sorter.sort(numbers);
        System.out.println("Asc");
        System.out.println(Arrays.toString(numbers));
        System.out.println();
    }
}
```

```
Original
[5, 4, 3, 2, 1]

    기준 i = 0
    초기 min = 0
    교환 min = 1
    교환 min = 2
    교환 min = 3
    교환 min = 4
    [1, 4, 3, 2, 5]
    --- 1 cycle --- 
    기준 i = 1
    초기 min = 1
    교환 min = 2
    교환 min = 3
    [1, 2, 3, 4, 5]
    --- 2 cycle --- 
    기준 i = 2
    초기 min = 2
    [1, 2, 3, 4, 5]
    --- 3 cycle --- 
    기준 i = 3
    초기 min = 3
    [1, 2, 3, 4, 5]
    --- 4 cycle --- 
Asc
[1, 2, 3, 4, 5]
```

## Binary Search

```java
package com.example.sorting;

import java.util.Arrays;

public class MyBinarySearch {

    public int search(int[] numbers, int value) {
        int low = 0;
        int high = numbers.length - 1;

        while (low <= high) {
            int mid = (low + high) / 2;
            System.out.printf("low=%d, high=%d, mid=%d ==> ", low, high, mid);

            if (value > numbers[mid]) {
                low = mid + 1;
            } else if (value < numbers[mid]) {
                high = mid - 1;
            } else {
                System.out.printf("대상 찾음 return %d\n", mid);
                return mid;
            }

            System.out.printf("low=%d, high=%d\n", low, high);
        }

        // 대상 찾지 못함
        return -1;
    }

    public static void main(String[] args) {
        int[] numbers = { 10, 8, 4, 6, 3, 5, 2, 7, 9, 1 };

        // 바이너리 서치는 대상 배열이 정렬되어 있다는 것을 전제로 한다.
        Arrays.sort(numbers);

        System.out.println("Target");
        System.out.println(Arrays.toString(numbers));
        System.out.println("\n");

        MyBinarySearch finder = new MyBinarySearch();

        int value = 7;
        System.out.println("value = " + value);
        System.out.println();

        int idx = finder.search(numbers, value);
        System.out.println();
        System.out.println("idx = " + idx);
    }
}
```

```
Target
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]


value = 7

low=0, high=9, mid=4 ==> low=5, high=9
low=5, high=9, mid=7 ==> low=5, high=6
low=5, high=6, mid=5 ==> low=6, high=6
low=6, high=6, mid=6 ==> 대상 찾음 return 6

idx = 6
```

## Shell Sort

```java
package com.example.sorting;

import java.util.Arrays;

public class MyShellSort2 {
    private int[] numbers;

    public void sort(int[] array) {
        numbers = array;

        int size = numbers.length;
        int interval = size / 2;

        while (interval >= 1) {
            for (int i = 0; i < interval; i++) {
                System.out.printf("intervalSort(%d, %d, %d)\n", i, size - 1, interval);
                System.out.println("------------------------------------------------");
                intervalSort(i, size - 1, interval);
                interval = interval / 2;
            }
        }
    }

    public void intervalSort(int begin, int end, int interval) {
        int j, item;

        for (int i = begin + interval; i <= end; i = i + interval) {
            System.out.printf("\tfor (int i = %d; %d <= %d; i = %d + %d)\n", i, i, end, i, interval);

            System.out.println("\t\t"+Arrays.toString(numbers));

            item = numbers[i];
            System.out.printf("\t\titem = %d\n", numbers[i]);

            System.out.printf("\t\t==> for (j = %d; %d >= %d && %d < %d; j = %d - %d)\n", 
                i - interval, i - interval, begin, item, numbers[i - interval], i - interval, interval);
            for (j = i - interval; j >= begin && item < numbers[j]; j = j - interval) {
                System.out.printf("\t\tfor (j = %d; %d >= %d && %d < %d; j = %d - %d)\n", 
                    j, j, begin, item, numbers[j], j, interval);

                numbers[j + interval] = numbers[j];
                System.out.printf("\t\t\tnumbers[%d] = numbers[%d]\n", j + interval, j);
                System.out.println();
            }

            numbers[j + interval] = item;
            System.out.printf("\t\tnumbers[%d] = %d\n", j + interval, item);

            System.out.println("\t\t"+Arrays.toString(numbers));
            System.out.println();
        }
    }

    public static void main(String[] args) {
        int[] numbers = { 5, 4, 3, 2, 1, 6 };

        MyShellSort2 sorter = new MyShellSort2();
        sorter.sort(numbers);
        System.out.println("------------------------------------------------");
        System.out.println(Arrays.toString(numbers) + "\n");
    }

}
```

```
intervalSort(0, 5, 3)
------------------------------------------------
    for (int i = 3; 3 <= 5; i = 3 + 3)
        [5, 4, 3, 2, 1, 6]
        item = 2
        ==> for (j = 0; 0 >= 0 && 2 < 5; j = 0 - 3)
        for (j = 0; 0 >= 0 && 2 < 5; j = 0 - 3)
            numbers[3] = numbers[0]

        numbers[0] = 2
        [2, 4, 3, 5, 1, 6]

intervalSort(0, 5, 1)
------------------------------------------------
    for (int i = 1; 1 <= 5; i = 1 + 1)
        [2, 4, 3, 5, 1, 6]
        item = 4
        ==> for (j = 0; 0 >= 0 && 4 < 2; j = 0 - 1)
        numbers[1] = 4
        [2, 4, 3, 5, 1, 6]

    for (int i = 2; 2 <= 5; i = 2 + 1)
        [2, 4, 3, 5, 1, 6]
        item = 3
        ==> for (j = 1; 1 >= 0 && 3 < 4; j = 1 - 1)
        for (j = 1; 1 >= 0 && 3 < 4; j = 1 - 1)
            numbers[2] = numbers[1]

        numbers[1] = 3
        [2, 3, 4, 5, 1, 6]

    for (int i = 3; 3 <= 5; i = 3 + 1)
        [2, 3, 4, 5, 1, 6]
        item = 5
        ==> for (j = 2; 2 >= 0 && 5 < 4; j = 2 - 1)
        numbers[3] = 5
        [2, 3, 4, 5, 1, 6]

    for (int i = 4; 4 <= 5; i = 4 + 1)
        [2, 3, 4, 5, 1, 6]
        item = 1
        ==> for (j = 3; 3 >= 0 && 1 < 5; j = 3 - 1)
        for (j = 3; 3 >= 0 && 1 < 5; j = 3 - 1)
            numbers[4] = numbers[3]

        for (j = 2; 2 >= 0 && 1 < 4; j = 2 - 1)
            numbers[3] = numbers[2]

        for (j = 1; 1 >= 0 && 1 < 3; j = 1 - 1)
            numbers[2] = numbers[1]

        for (j = 0; 0 >= 0 && 1 < 2; j = 0 - 1)
            numbers[1] = numbers[0]

        numbers[0] = 1
        [1, 2, 3, 4, 5, 6]

    for (int i = 5; 5 <= 5; i = 5 + 1)
        [1, 2, 3, 4, 5, 6]
        item = 6
        ==> for (j = 4; 4 >= 0 && 6 < 5; j = 4 - 1)
        numbers[5] = 6
        [1, 2, 3, 4, 5, 6]

------------------------------------------------
[1, 2, 3, 4, 5, 6]
```



