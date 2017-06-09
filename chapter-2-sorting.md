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

셸 정렬은 주어진 자료 리스트를 특정 매개변수 값의 길이를 갖는 부파일로 쪼개서, 각 부파일에서 정렬을 수행한다. 즉, 매개변수 값에 따라 부파일이 발생하며, 매개변수값을 줄이며 이 과정을 반복하고 결국 매개변수 값이 1이면 정렬은 완성된다.

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

## Quick Sort

퀵 정렬은 n개의 데이터를 정렬할 때, 최악의 경우에는 O\(n2\)번의 비교를 수행하고, 평균적으로 O\(n log n\)번의 비교를 수행한다.  
퀵 정렬의 내부 루프는 대부분의 컴퓨터 아키텍처에서 효율적으로 작동하도록 설계되어 있고\(그 이유는 메모리 참조가 지역화되어 있기 때문에 CPU 캐시의 히트율이 높아지기 때문이다.\), 대부분의 실질적인 데이터를 정렬할 때 제곱 시간이 걸릴 확률이 거의 없도록 알고리즘을 설계하는 것이 가능하다. 때문에 일반적인 경우 퀵 정렬은 다른 O\(n log n\) 알고리즘에 비해 훨씬 빠르게 동작한다.

퀵 정렬은 분할 정복\(divide and conquer\) 방법을 통해 리스트를 정렬한다. 리스트 가운데서 하나의 원소를 고른다. 이렇게 고른 원소를 피벗이라고 한다. 피벗 앞에는 피벗보다 값이 작은 모든 원소들이 오고, 피벗 뒤에는 피벗보다 값이 큰 모든 원소들이 오도록 피벗을 기준으로 리스트를 둘로 나눈다. 이렇게 리스트를 둘로 나누는 것을 분할이라고 한다. 분할을 마친 뒤에 피벗은 더 이상 움직이지 않는다. 분할된 두 개의 작은 리스트에 대해 재귀\(Recursion\)적으로 이 과정을 반복한다. 재귀는 리스트의 크기가 0이나 1이 될 때까지 반복된다. 재귀 호출이 한번 진행될 때마다 최소한 하나의 원소는 최종적으로 위치가 정해지므로, 이 알고리즘은 반드시 끝난다는 것을 보장할 수 있다.

```java
package com.example.sorting;

import java.util.Arrays;

public class MyQuickSort {
    private int[] numbers;

    private void sort(int[] numbers) {
        this.numbers = numbers;

        System.out.printf("sort(%d, %d)\n", 0, numbers.length - 1);
        sort(0, numbers.length - 1, 0);
    }

    public void sort(int left, int right, int depth) {
        System.out.println("-------------------------------------- 시작");
        System.out.println(Arrays.toString(numbers));

        int leftHolder = left;
        int rightHolder = right;
        int low = left;
        int high = right;
        int pivot = numbers[left];

        while (low < high) {
            // 피봇보다 작은 숫자 찾기
            while (low < high && pivot <= numbers[high]) {
                System.out.println("high--");
                high--;
            }

            if (high != low) {
                numbers[low] = numbers[high];
                System.out.println("피봇보다 작은 숫자 찾기\t" + Arrays.toString(numbers));
            }

            // 피봇보다 큰 숫자 찾기
            while (low < high && pivot >= numbers[low]) {
                System.out.println("low++");
                low++;
            }

            if (low != high) {
                numbers[high] = numbers[low];
                System.out.println("피봇보다 큰 숫자 찾기\t" + Arrays.toString(numbers));
            }

            System.out.printf("------- low=%d < high=%d -------\n", low, high);
        }

        numbers[low] = pivot;
        /*
         * 이제, 피봇보다 작은 값은 왼쪽에, 큰 값은 오른쪽에 위치한다.
         */
        System.out.println(Arrays.toString(numbers));
        System.out.printf("leftHolder=%d, rightHolder=%d, low=%d, high=%d\n", 
            leftHolder, rightHolder, low, high);
        System.out.println("-------------------------------------- 종료");
        System.out.println();

        String taps = getTaps(depth);

        if (leftHolder < low) {
            // 피봇보다 작은 숫자들을 대상으로 정렬을 진행한다.
            System.out.printf(taps + "<< sort(%d, %d)\n", leftHolder, low - 1);
            sort(leftHolder, low - 1, depth + 1);
        } else {
            System.out.println(taps + "<< [ x ]");
        }

        if (low < rightHolder) {
            // 피봇보다 큰 숫자들을 대상으로 정렬을 진행한다.
            System.out.printf(taps + ">> sort(%d, %d)\n", low + 1, rightHolder);
            sort(low + 1, rightHolder, depth + 1);
        } else {
            System.out.println(taps + ">> [ x ]");
        }

    }

    private String getTaps(int depth) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i <= depth; i++) {
            sb.append("\t");
        }
        return sb.toString();
    }

    public static void main(String[] args) {
        int[] numbers = { 3, 5, 2, 4, 1 };
        System.out.println("Original");
        System.out.println(Arrays.toString(numbers));
        System.out.println("\n");

        MyQuickSort sorter = new MyQuickSort();
        sorter.sort(numbers);

        System.out.println("\n");
        System.out.println("==========================================");
        System.out.println("Asc");
        System.out.println(Arrays.toString(numbers));
        System.out.println("\n");
    }

}
```

```
Original
[3, 5, 2, 4, 1]


sort(0, 4)
-------------------------------------- 시작
[3, 5, 2, 4, 1]
피봇보다 작은 숫자 찾기    [1, 5, 2, 4, 1]
low++
피봇보다 큰 숫자 찾기    [1, 5, 2, 4, 5]
------- low=1 < high=4 -------
high--
high--
피봇보다 작은 숫자 찾기    [1, 2, 2, 4, 5]
low++
------- low=2 < high=2 -------
[1, 2, 3, 4, 5]
leftHolder=0, rightHolder=4, low=2, high=2
-------------------------------------- 종료

    << sort(0, 1)
-------------------------------------- 시작
[1, 2, 3, 4, 5]
high--
------- low=0 < high=0 -------
[1, 2, 3, 4, 5]
leftHolder=0, rightHolder=1, low=0, high=0
-------------------------------------- 종료

        << [ x ]
        >> sort(1, 1)
-------------------------------------- 시작
[1, 2, 3, 4, 5]
[1, 2, 3, 4, 5]
leftHolder=1, rightHolder=1, low=1, high=1
-------------------------------------- 종료

            << [ x ]
            >> [ x ]
    >> sort(3, 4)
-------------------------------------- 시작
[1, 2, 3, 4, 5]
high--
------- low=3 < high=3 -------
[1, 2, 3, 4, 5]
leftHolder=3, rightHolder=4, low=3, high=3
-------------------------------------- 종료

        << [ x ]
        >> sort(4, 4)
-------------------------------------- 시작
[1, 2, 3, 4, 5]
[1, 2, 3, 4, 5]
leftHolder=4, rightHolder=4, low=4, high=4
-------------------------------------- 종료

            << [ x ]
            >> [ x ]


==========================================
Asc
[1, 2, 3, 4, 5]
```

## Merge Sort

합병 정렬은 다음과 같이 작동한다. 리스트의 길이가 0 또는 1이면 이미 정렬된 것으로 본다. 그렇지 않은 경우에는 정렬되지 않은 리스트를 절반으로 잘라 비슷한 크기의 두 부분 리스트로 나눈다. 각 부분 리스트를 재귀적으로 합병 정렬을 이용해 정렬한다. 두 부분 리스트를 다시 하나의 정렬된 리스트로 합병한다.

```java
package com.example.sorting;

import java.util.Arrays;

public class MyMergeSort2 {

    private int[] numbers;

    private void sort(int[] numbers) {
        this.numbers = numbers;

        System.out.printf("divide(%d, %d)\n", 0, numbers.length - 1);

        int depth = 0;
        divide(0, numbers.length - 1, depth);
    }

    private void divide(int left, int right, int depth) {
        String taps = getTaps(depth);

        if (left < right) {
            int mid = (left + right) / 2;

            System.out.printf(taps + "<< divide(%d, %d)\n", left, mid);
            divide(left, mid, depth + 1);

            System.out.printf(taps + ">> divide(%d, %d)\n", mid + 1, right);
            divide(mid + 1, right, depth + 1);

            System.out.printf(taps + "++ merge(left=%d, mid=%d, right=%d)\n", left, mid, right);
            merge(left, mid, right, depth);
        }
    }

    private void merge(int left, int mid, int right, int depth) {
        String taps = getTaps(depth);

        // 작업대상 중 왼쪽 부분의 시작을 가리키는 인덱스
        int i = left;
        // 작업대상 중 오른쪽 부분의 시작을 가리키는 인덱스
        int j = mid + 1;
        // 임시배열에 얼마나 옮겼는지 체크하는 인덱스
        int k = left;

        int[] tempArray = new int[numbers.length];

        System.out.println(taps + "+----------------+ 작은 값을 임시배열에 담기");
        System.out.printf(taps + "%s, i=%d, j=mid+1=%d, k=%d\n", Arrays.toString(numbers), i, j, k);
        while (i <= mid && j <= right) {
            // 작은 값을 임시배열에 넣는다.
            System.out.printf(taps + "if (numbers[%d]=%d < numbers[%d]=%d)\n", 
                i, numbers[i], j, numbers[j]);
            if (numbers[i] < numbers[j]) {
                tempArray[k] = numbers[i];
                System.out.printf(taps + "%s, i++\n", Arrays.toString(tempArray));
                i++;
            } else {
                tempArray[k] = numbers[j];
                System.out.printf(taps + "%s, j++\n", Arrays.toString(tempArray));
                j++;
            }
            k++;
            System.out.println(taps + "k++");
            System.out.println(taps + "------------------");
            System.out.printf(taps + "%s, i=%d, j=%d, k=%d\n", Arrays.toString(tempArray), i, j, k);
        }
        System.out.println(taps + "+----------------+ 작은 값을 임시배열에 담기 작업 끝");
        System.out.printf(taps + "%s, i=%d, j=%d, k=%d, mid=%d\n", Arrays.toString(tempArray), i, j, k, mid);

        System.out.println(taps + "================== 나머지 임시배열에 담기");
        // 값이 커서 넣지 못한 값을 임시배열에 넣는다.
        // i > mid 라는 것은 상대적으로 j가 가리키는 값을 넣지 못했다는 것을 의미한다.
        System.out.printf(taps + "if (i=%d > mid=%d)\n", i, mid);
        if (i > mid) {
            for (int m = j; m <= right; m++) {
                tempArray[k] = numbers[m];
                k++;

                System.out.println(taps + "k++");
                System.out.printf(taps + "오른쪽 %s\n", Arrays.toString(tempArray));
            }
        } else {
            for (int m = i; m <= mid; m++) {
                tempArray[k] = numbers[m];
                k++;

                System.out.println(taps + "k++");
                System.out.printf(taps + "왼 쪽 %s\n", Arrays.toString(tempArray));
            }
        }
        System.out.println(taps + "~~~~~~~~~~~~~~~~~~ 정렬된 임시배열 값을 타겟배열에 담기");

        // 임시배열의 값을 타겟배열에 덮어쓴다.
        for (int m = left; m <= right; m++) {
            numbers[m] = tempArray[m];
        }

        System.out.printf(taps + "%s\n", Arrays.toString(numbers));
    }

    private String getTaps(int depth) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i <= depth; i++) {
            sb.append("\t");
        }
        return sb.toString();
    }

    public static void main(String[] args) {
        int[] numbers = { 3, 5, 2, 4, 1 };

        System.out.println("Original");
        System.out.println(Arrays.toString(numbers));
        System.out.println("\n");

        MyMergeSort2 sorter = new MyMergeSort2();
        sorter.sort(numbers);

        System.out.println("\n");
        System.out.println("Asc");
        System.out.println(Arrays.toString(numbers));
        System.out.println("\n");
    }

}
```

```
Original
[3, 5, 2, 4, 1]


divide(0, 4)
    << divide(0, 2)
        << divide(0, 1)
            << divide(0, 0)
            >> divide(1, 1)
            ++ merge(left=0, mid=0, right=1)
            +----------------+ 작은 값을 임시배열에 담기
            [3, 5, 2, 4, 1], i=0, j=mid+1=1, k=0
            if (numbers[0]=3 < numbers[1]=5)
            [3, 0, 0, 0, 0], i++
            k++
            ------------------
            [3, 0, 0, 0, 0], i=1, j=1, k=1
            +----------------+ 작은 값을 임시배열에 담기 작업 끝
            [3, 0, 0, 0, 0], i=1, j=1, k=1, mid=0
            ================== 나머지 임시배열에 담기
            if (i=1 > mid=0)
            k++
            오른쪽 [3, 5, 0, 0, 0]
            ~~~~~~~~~~~~~~~~~~ 정렬된 임시배열 값을 타겟배열에 담기
            [3, 5, 2, 4, 1]
        >> divide(2, 2)
        ++ merge(left=0, mid=1, right=2)
        +----------------+ 작은 값을 임시배열에 담기
        [3, 5, 2, 4, 1], i=0, j=mid+1=2, k=0
        if (numbers[0]=3 < numbers[2]=2)
        [2, 0, 0, 0, 0], j++
        k++
        ------------------
        [2, 0, 0, 0, 0], i=0, j=3, k=1
        +----------------+ 작은 값을 임시배열에 담기 작업 끝
        [2, 0, 0, 0, 0], i=0, j=3, k=1, mid=1
        ================== 나머지 임시배열에 담기
        if (i=0 > mid=1)
        k++
        왼 쪽 [2, 3, 0, 0, 0]
        k++
        왼 쪽 [2, 3, 5, 0, 0]
        ~~~~~~~~~~~~~~~~~~ 정렬된 임시배열 값을 타겟배열에 담기
        [2, 3, 5, 4, 1]
    >> divide(3, 4)
        << divide(3, 3)
        >> divide(4, 4)
        ++ merge(left=3, mid=3, right=4)
        +----------------+ 작은 값을 임시배열에 담기
        [2, 3, 5, 4, 1], i=3, j=mid+1=4, k=3
        if (numbers[3]=4 < numbers[4]=1)
        [0, 0, 0, 1, 0], j++
        k++
        ------------------
        [0, 0, 0, 1, 0], i=3, j=5, k=4
        +----------------+ 작은 값을 임시배열에 담기 작업 끝
        [0, 0, 0, 1, 0], i=3, j=5, k=4, mid=3
        ================== 나머지 임시배열에 담기
        if (i=3 > mid=3)
        k++
        왼 쪽 [0, 0, 0, 1, 4]
        ~~~~~~~~~~~~~~~~~~ 정렬된 임시배열 값을 타겟배열에 담기
        [2, 3, 5, 1, 4]
    ++ merge(left=0, mid=2, right=4)
    +----------------+ 작은 값을 임시배열에 담기
    [2, 3, 5, 1, 4], i=0, j=mid+1=3, k=0
    if (numbers[0]=2 < numbers[3]=1)
    [1, 0, 0, 0, 0], j++
    k++
    ------------------
    [1, 0, 0, 0, 0], i=0, j=4, k=1
    if (numbers[0]=2 < numbers[4]=4)
    [1, 2, 0, 0, 0], i++
    k++
    ------------------
    [1, 2, 0, 0, 0], i=1, j=4, k=2
    if (numbers[1]=3 < numbers[4]=4)
    [1, 2, 3, 0, 0], i++
    k++
    ------------------
    [1, 2, 3, 0, 0], i=2, j=4, k=3
    if (numbers[2]=5 < numbers[4]=4)
    [1, 2, 3, 4, 0], j++
    k++
    ------------------
    [1, 2, 3, 4, 0], i=2, j=5, k=4
    +----------------+ 작은 값을 임시배열에 담기 작업 끝
    [1, 2, 3, 4, 0], i=2, j=5, k=4, mid=2
    ================== 나머지 임시배열에 담기
    if (i=2 > mid=2)
    k++
    왼 쪽 [1, 2, 3, 4, 5]
    ~~~~~~~~~~~~~~~~~~ 정렬된 임시배열 값을 타겟배열에 담기
    [1, 2, 3, 4, 5]


Asc
[1, 2, 3, 4, 5]
```



