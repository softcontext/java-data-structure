# 빅오\(Big-O\)표기법

```java
public class Test {
	/*
	 * 참고: [자료구조 알고리즘] 빅오(Big-O)표기법 완전정복 / https://www.youtube.com/watch?v=6Iq5iMCVsXA
	 * 
	 * Big-O: 알고리즘의 성능을 수학적으로 표기해주는 표기법, 빅오표기법으로 시간 및 공간 복잡도를 표기할 수 있다.
	 * 데이터 또는 사용자의 요구의 증가에 따른 처리시간의 증가를 예측하는 것이 목적이다.
	 * 
	 * [빅오 그래프](https://mahmutben.files.wordpress.com/2016/06/12.png)
	 */

	public static void main(String[] args) {
		int[] nums = {0, 2, 4};
		int[][] numbers = {
				nums,
				{1, 3, 5, 7}
		};
		int[] arr = {1,2,3,4,5,6,7,8,9,10};
		
		System.out.println(o1(nums));
		
		oN(nums);
		
		oNPower2(numbers);
		
		oNM(nums, arr);
		
		oNPower3(nums);
		
		o2PowerN(5);
		
		oLogN(arr, 6);
		
		oSqrtN(16);
	}

	/*
	 * O(sqrt(n))
	 */
	private static void oSqrtN(int n) {
		for (int j = 0; j < Math.sqrt(n); j++) {
			System.out.print(j + " ");
		}
		System.out.println();
	}

	/*
	 * O(logN)
	 * 반복문의 1회전 처리마다 대상 데이타가 1/2씩 줄어드는 특징이 있다.
	 * 처리대상 데이타가 증가해도 처리속도가 크게 증가되지 않는다.
	 */
	private static void oLogN(int[] arr, int target) {
		int idx = search(arr, target);
		System.out.println("idx = " + idx);
	}
	
	public static int search(int[] numbers, int value) {
		int low = 0;
		int high = numbers.length - 1;

		while (low <= high) {
			int mid = (low + high) / 2;

			if (value > numbers[mid]) {
				low = mid + 1;
			} else if (value < numbers[mid]) {
				high = mid - 1;
			} else {
				return mid;
			}
		}

		// 대상 찾지 못함
		return -1;
	}

	/*
	 * O(2^n)
	 * n^3 보다 더 많은 처리시간이 필요하다. 
	 * 
	 * 대표적인 2^n 알고리즘으로 피보나치 수열이 있다.
	 * f(n) = f(n-1) + f(n-2), n >= 2 일 때
	 * 
	 * n이 2일 때 리프의 개수는 2개 = 2
	 * n이 3일 때 리프의 개수는 4개 = 2 x 2
	 * n이 4일 때 리프의 개수는 8개 = 2 x 2 x 2 ==> 2^n
	 * 
	 * n이 증가할 때마다 리프의 개수가 2배로 증가한다.
	 */
	private static void o2PowerN(int n) {
		int[] result  = getFibo(n);
		System.out.println(Arrays.toString(result));
		
		long value = fibo(n);
		System.out.println(value); // 5
		
		/*
		 * 피보나치 알고리즘은 반드시 모든 노드를 거쳐야 하는 것이 아니므로 
		 * 재귀호출 로직을 반복문 로직으로 바꾸어 처리속도를 개선할 수 있다.
		 */
		
		value = fiboBetter(n);
		System.out.println(value); // 5
	}

	private static long fiboBetter(int n) {
		int a = 1;
		int b = 1;
		n = n - 2;
		int sum = 0;
		
		if (n <= 2) return 1;
		
		for (int i = 0; i < n; i++) {
			sum = a + b;
			a = b;
			b = sum;
		}

		return sum;
	}

	private static long fibo(int n) {
		if (n <= 1) {
			return n;
		} 
		return fibo(n - 1) + fibo(n - 2);  // [1, 1, 2, 3, 5]
		/*
		 * [(5) -> 
		 * 		[(4) -> 
		 * 			[(3) -> 
		 * 				[(2) -> 
		 * 					[(1) -> 
		 * 						return 1
		 * 					] + 
		 * 					[(0) -> 
		 * 						return 0
		 * 					]
		 * 				] + 
		 * 				[(1) -> 
		 * 					return 1
		 * 				]
		 * 			] + 
		 * 			[(2) -> 
		 * 				[(1) -> 
		 * 					return 1
		 * 				] + 
		 * 				[(0) -> 
		 * 					return 0
		 * 				]
		 * 			]
		 * 		] + 
		 * 		[(3) -> 
		 * 			[(2) -> 
		 * 				[(1) -> 
		 * 					return 1
		 * 				] + 
		 * 				[(0) -> 
		 * 					return 0
		 * 				]
		 * 			] + 
		 * 			[(1) -> 
		 * 				return 1
		 * 			]
		 * 		]
		 * ]
		 * 
		 * 파라미터를 2로 주고 함수를 호출하는 경우가 3번 발생하고 있다.
		 * 이는 중복 처리하는 것으로 속도를 떨어뜨리는 원인이 되므로 이 알고리즘은 좋지 않다.
		 */
	}

	public static int[] getFibo(int count) {
		int[] numbers = new int[count];
		// 시드값 1, 1 을 할당한다.
		numbers[0] = 1;
		numbers[1] = 1;

		// 번호 2개는 이미 할당했으므로 -2를 한다.
		seekFibo(numbers, count - 2, numbers[0], numbers[1], 2);
		return numbers;
	}
	
	private static void seekFibo(int[] numbers, int count, int n1, int n2, int idx) {
		if (count > 0) {
			numbers[idx] = n1 + n2;

			n1 = n2;
			n2 = numbers[idx];
			idx++;
			seekFibo(numbers, count - 1, n1, n2, idx);
		}
	}

	/*
	 * O(n^3)
	 * 삼중 for문의 형태를 보여준다. 3차원 배열의 데이터를 처리할 때 발생하며 n^2 보다 더 많은 처리시간이 필요하다.
	 */
	private static void oNPower3(int[] nums) {
		for (int i = 0; i < nums.length; i++) {
			for (int j = 0; j < nums.length; j++) {
				for (int k = 0; k < nums.length; k++) {
					System.out.print(i + j + k);
					System.out.print(k < nums.length - 1 ? "," : "");
				}
				System.out.print(j < nums.length - 1 ? "|" : "");
			}
			System.out.print(i < nums.length - 1 ? "~~~" : "");
		}
		System.out.println();
	}

	/*
	 * O(nm)
	 * O(n^2)과 비슷하다. 하지만 n이 작고 m이 큰경우 O(n^2)보다 처리시간이 적게 소요되므로 구분하는 것이 좋다.
	 */
	private static void oNM(int[] n, int[] m) {
		for (int i = 0; i < n.length; i++) {
			for (int j = 0; j < m.length; j++) {
				System.out.print((i + j) + " ");
			}
		}
		System.out.println();
	}

	/*
	 * O(n^2)
	 * 데이터가 늘어날수록 처리시간이 크게 증가한다.
	 */
	private static void oNPower2(int[][] numbers) {
		for (int i = 0; i < numbers.length; i++) {
			for (int j = 0; j < numbers[i].length; j++) {
				System.out.print(numbers[i][j]);
			}
			System.out.print(" ");
		}
		System.out.println();
	}

	/*
	 * O(n)
	 * 입력 데이터의 크기에 비례해서 처리시간이 증가한다.
	 */
	private static void oN(int[] n) {
		for (int i = 0; i < n.length; i++) {
			System.out.print(n[i]);
		}
		System.out.println();
	}

	/*
	 * O(1)
	 * 배열의 첫 요소가 0인지의 여부를 판단한다.
	 * 파라미터로 받는 배열의 크기와 상관없이 언제나 일정한 시간만이 소요된다.
	 */
	private static boolean o1(int[] n) {
		return n[0] == 0 ? true : false;
	}

}
```



