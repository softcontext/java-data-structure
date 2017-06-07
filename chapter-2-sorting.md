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

Insertion Sort

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


