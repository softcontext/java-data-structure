# Chapter 3. Collection

## Array Rotate

```java
package com.example.sample0array;

public class ArrayRotate {
	private static final int ROTATE_LEFT = -1;
	private static final int ROTATE_RIGHT = 1;
	private int[][] nums;
	private int[][] temp;
	
	public static void main(String[] args) {
		ArrayRotate rotater = new ArrayRotate();
		
		rotater.createArray(3);
		rotater.printArray();
		
		System.out.println("  90도 좌회전");
		rotater.rotate(ROTATE_LEFT);
		rotater.printArray();
		
		System.out.println("  90도 좌회전");
		rotater.rotate(ROTATE_LEFT);
		rotater.printArray();
		
		System.out.println("  90도 좌회전");
		rotater.rotate(ROTATE_LEFT);
		rotater.printArray();
		
		System.out.println("  90도 좌회전");
		rotater.rotate(ROTATE_LEFT);
		rotater.printArray();
		
		System.out.println("  90도 우회전");
		rotater.rotate(ROTATE_RIGHT);
		rotater.printArray();
		
		System.out.println("  90도 우회전");
		rotater.rotate(ROTATE_RIGHT);
		rotater.printArray();
		
		System.out.println("  90도 우회전");
		rotater.rotate(ROTATE_RIGHT);
		rotater.printArray();
		
		System.out.println("  90도 우회전");
		rotater.rotate(ROTATE_RIGHT);
		rotater.printArray();
	}

	private void rotate(int direction) {
		if (direction == -1) {
			rotateLeft();
		} else {
			rotateRight();
		}
	}

	/*
	 * 	0.0		0.1		0.2		==>		2.0		1.0		0.0
	 */
	private void rotateRight() {
		temp = new int[nums.length][nums.length];
		
		for (int i = 0; i < nums.length; i++) {
			int x = nums.length-1;
			for (int j = 0; j < nums.length; j++) {
				temp[i][j] = nums[x][i];
				x--;
			}
		}
		
		nums = temp;
	}

	/*
	 * 	0.0		0.1		0.2		==>		0.2		1.2		2.2
	 */
	private void rotateLeft() {
		temp = new int[nums.length][nums.length];
		
		for (int i = 0; i < nums.length; i++) {
			for (int j = 0; j < nums.length; j++) {
				temp[i][j] = nums[j][nums.length-1-i];
			}
		}
		
		nums = temp;
	}

	private void printArray() {
		if (nums != null) {
			for (int i = 0; i < nums.length; i++) {
				for (int j = 0; j < nums.length; j++) {
					System.out.printf("%3d", nums[i][j]);
				}
				System.out.println();
			}
			System.out.println("  -------   ");
		}
	}

	private void createArray(int size) {
		int count = 1;
		nums = new int[size][size];
		
		for (int i = 0; i < nums.length; i++) {
			for (int j = 0; j < nums.length; j++) {
				nums[i][j] = count;
				count++;
			}
		}
	}

}

```

```
  1  2  3
  4  5  6
  7  8  9
  -------   
  90도 좌회전
  3  6  9
  2  5  8
  1  4  7
  -------   
  90도 좌회전
  9  8  7
  6  5  4
  3  2  1
  -------   
  90도 좌회전
  7  4  1
  8  5  2
  9  6  3
  -------   
  90도 좌회전
  1  2  3
  4  5  6
  7  8  9
  -------   
  90도 우회전
  7  4  1
  8  5  2
  9  6  3
  -------   
  90도 우회전
  9  8  7
  6  5  4
  3  2  1
  -------   
  90도 우회전
  3  6  9
  2  5  8
  1  4  7
  -------   
  90도 우회전
  1  2  3
  4  5  6
  7  8  9
  -------   

```



