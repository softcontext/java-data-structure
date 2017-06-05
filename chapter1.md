# Chapter 1. Recursive

## 입문

Recursive Example 1

```java
package com.example.recursive;

public class Rec1 {

    static void p() throws InterruptedException {
        System.out.println("hello");
        Thread.sleep(500);

        // 메소드 자신을 재 호출하는 것은 무한루프에 빠뜨린다.
        // 메소드 호출 시 마다 메소드 로직 처리를 위한 스택이 매번 생성된다.
        // 스택의 개수는 한계가 있으므로 결국 StackOverflowError가 발생한다.
        p();
    }

    public static void main(String[] args) throws InterruptedException {
        // 촉발(trigger)
        p();
    }

}
```

```console
hello
hello
hello
hello
hello
hello
hello
...
```

Recursive Example 2

```java
package com.example.recursive;

public class Rec2 {
    // 스택이 몇개 만들어지는지 알려주는 도구는 없다. 대신 직접 메소드 호출 횟수를 카운트한다.
    static int count = 0;

    static void p() throws InterruptedException {
        // 재귀호출은 무한루프에 빠지지 않기 위한 제어가 필요하다.
        if (count < 5) {
            count++;
            System.out.println("hello " + count);
            Thread.sleep(500);
            p();
        }
    }

    public static void main(String[] args) throws InterruptedException {
        p();
    }
}
```

```console
hello 1
hello 2
hello 3
hello 4
hello 5
```

Recursive Example 3

```java
package com.example.recursive;

public class Rec3 {

    static int factorial(int n, int depth) {
        if (n == 1) {
            System.out.println(getDepth(depth) + "return 1");
            return 1;
        } else {
            System.out.printf(getDepth(depth) + "return (%d * factorial(%d, %d))\n", n, n - 1, depth + 1);
            return (n * factorial(n - 1, depth + 1));
        }
    }

    // 스택의 선후관계를 파악하기 위해서 들여쓰기를 한다.
    private static String getDepth(int depth) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < depth; i++) {
            sb.append("\t");
        }
        return sb.toString();
    }

    public static void main(String[] args) {
        int value = 4;
        int depth = 1;

        System.out.printf(getDepth(depth) + "factorial(%d, %d)\n", value, depth);
        int result = factorial(value, depth);
        System.out.println("result = " + result);
    }
}
```

```console
    factorial(4, 1)
    return (4 * factorial(3, 2))
        return (3 * factorial(2, 3))
            return (2 * factorial(1, 4))
                return 1
result = 24
```

Recursive Example 4 : Fibonacci Numbers

```java
package com.example.recursive;

import java.util.Arrays;

public class Rec4 {
    static int n1 = 0, n2 = 1;
    static int n3 = 0;

    static void fibo(int count) {
        // 0 and 1 출력
        System.out.print(n1 + " " + n2);

        // 번호 2개는 이미 출력했으므로 -2를 한다.
        printFibo(count - 2);

        System.out.println();
    }

    static void printFibo(int count) {
        if (count > 0) {
            // 앞 2개의 수를 더해서 3번째 수를 구한다음 출력한다.
            n3 = n1 + n2;
            System.out.print(" " + n3);

            // 다음 작업을 위해 변수의 값을 바꾼다.
            n1 = n2;
            n2 = n3;
            printFibo(count - 1);
        }
    }

    public int[] getFibo(int count) {
        int[] numbers = new int[count];
        // 시드값 0, 1 을 할당한다.
        numbers[0] = 0;
        numbers[1] = 1;

        // 번호 2개는 이미 할당했으므로 -2를 한다.
        seekFibo(numbers, count - 2, 0, 1, 2);
        return numbers;
    }

    private void seekFibo(int[] numbers, int count, int n1, int n2, int idx) {
        if (count > 0) {
            numbers[idx] = n1 + n2;

            n1 = n2;
            n2 = numbers[idx];
            seekFibo(numbers, count - 1, n1, n2, idx + 1);
        }
    }

    public static void main(String[] args) {
        int count = 15;

        fibo(count);
        // 0 1 1 2 3 5 8 13 21 34 55 89 144 233 377

        Rec4 seeker = new Rec4();
        int[] numbers = seeker.getFibo(count);
        System.out.println(Arrays.toString(numbers));
        // [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377]
    }
}
```

```console
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377]
```

Recursive Example 5

```java
package com.example.recursive;

public class Rec5 {

    static int factorial(int number) {
        if (number == 0) {
            return 1;
        }

        int sum = 1;
        int depth = 1;

        System.out.printf(getDepth(depth) + "return factorialLoop(%d, %d, %d)\n", number, sum, depth + 1);
        return factorialLoop(number, sum, depth + 1);
    }

    static int factorialLoop(int currentNumber, int sum, int depth) {
        if (currentNumber == 1) {
            System.out.println(getDepth(depth) + "return " + sum);
            return sum;
        } else {
            /*
             * 4! == 1 * 2 * 3 * 4 == 1 * 4 * 3 * 2 == 24
             * factorialLoop 메소드가 리턴하는 결과를 그대로 다시 리턴하므로
             * 메소드 호출 시 전달되는 파라미터 sum은 메소드 호출 시 마다 처리된 중간 값이다.
             */
            System.out.printf(getDepth(depth) + "return factorialLoop(%d, %d, %d)\n", 
                currentNumber - 1, sum * currentNumber, depth + 1);
            return factorialLoop(currentNumber - 1, sum * currentNumber, depth + 1);
        }
    }

    static String getDepth(int depth) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < depth; i++) {
            sb.append("\t");
        }
        return sb.toString();
    }

    public static void main(String[] args) {
        int number = 4;
        int depth = 1;
        System.out.printf(getDepth(depth) + "factorial(%d)\n", number);
        System.out.printf("%d! = %d", number, factorial(number));
    }

}
```

```java
    factorial(4)
    return factorialLoop(4, 1, 2)
        return factorialLoop(3, 4, 3)
            return factorialLoop(2, 12, 4)
                return factorialLoop(1, 24, 5)
                    return 24
4! = 24
```

## 분류

Single Recursion

```java
package com.example.recursive2;

public class SingleRecursion {

    public static long factorial(int num) {
        if (num < 0) {
            throw new IllegalArgumentException("Can't calculate factorial of negative");
        }

        if (num < 2) {
            System.out.println("return 1");
            return 1;
        } else {
            System.out.printf("return %d * factorial(%d)\n", num, num - 1);
            return num * factorial(num - 1);
        }
    }

    public static void main(String[] args) {
        int num = 4;
        System.out.printf("호출 factorial(%d)\n", num);
        System.out.printf("결과 %d! = %d", num, factorial(num));
    }

}
```

```console
호출 factorial(4)
return 4 * factorial(3)
return 3 * factorial(2)
return 2 * factorial(1)
return 1
결과 4! = 24
```

Multiple Recursion

```java
package com.example.recursive2;

public class MultipleRecursion {

    public static long fibonacci(String flag, long idx) {
        if (idx < 0) {
            throw new IllegalArgumentException("Can't accept negative arguments");
        }

        if (idx < 2) {
            System.out.println(flag + " return " + idx);
            return idx;
        } else {
            // 메소드 호출 후 로직이 처리될 때 사용되는 지역변수는 개별적인 스택을 이용하므로
            // 각각 메소드 호출마다 변수의 값은 다르고 메소드들 끼리 서로에게 영향을 미치지 않는다.
            // 메소드 호출은 왼쪽부터 처리된다. 왼쪽 메소드가 촉발한 메소드 호출 체인이 끝나면 오른쪽 메소드가 호출된다.
            System.out.printf("fibonacci(\"<<\", %d) + fibonacci(\">>\", %d)\n", idx - 1, idx - 2);
            return fibonacci("<<", idx - 1) + fibonacci(">>", idx - 2);
        }
    }

    public static void main(String[] args) {
        /*
         * 0 1 1 2 3 5 ...
         */
        int max = 6;
        for (int i = 0; i < max; i++) {
            System.out.printf("호출 fibonacci(\"--\", %d)\n", i);
            long x = fibonacci("--", i);
            System.out.println("결과 " + x + "\n");
        }
    }
}
```

```console
호출 fibonacci("--", 0)
-- return 0
결과 0

호출 fibonacci("--", 1)
-- return 1
결과 1

호출 fibonacci("--", 2)
fibonacci("<<", 1) + fibonacci(">>", 0)
<< return 1
>> return 0
결과 1

호출 fibonacci("--", 3)
fibonacci("<<", 2) + fibonacci(">>", 1)
fibonacci("<<", 1) + fibonacci(">>", 0)
<< return 1
>> return 0
>> return 1
결과 2

호출 fibonacci("--", 4)
fibonacci("<<", 3) + fibonacci(">>", 2)
fibonacci("<<", 2) + fibonacci(">>", 1)
fibonacci("<<", 1) + fibonacci(">>", 0)
<< return 1
>> return 0
>> return 1
fibonacci("<<", 1) + fibonacci(">>", 0)
<< return 1
>> return 0
결과 3

호출 fibonacci("--", 5)
fibonacci("<<", 4) + fibonacci(">>", 3)
fibonacci("<<", 3) + fibonacci(">>", 2)
fibonacci("<<", 2) + fibonacci(">>", 1)
fibonacci("<<", 1) + fibonacci(">>", 0)
<< return 1
>> return 0
>> return 1
fibonacci("<<", 1) + fibonacci(">>", 0)
<< return 1
>> return 0
fibonacci("<<", 2) + fibonacci(">>", 1)
fibonacci("<<", 1) + fibonacci(">>", 0)
<< return 1
>> return 0
>> return 1
결과 5
```

Mutual Recursion

```java
package com.example.recursive2;

public class MutualRecursion {

    public static boolean isOdd(int num) {
        if (num < 0) {
            throw new IllegalArgumentException("Can't accept negative arguments");
        }

        if (num == 0) {
            System.out.println("return false");
        } else {
            System.out.printf("호출 isEven(%d)\n", num - 1);
        }
        return (num == 0) ? false : isEven(num - 1);
    }

    public static boolean isEven(int num) {
        if (num < 0) {
            throw new IllegalArgumentException("Can't accept negative arguments");
        }

        if (num == 0) {
            System.out.println("return true");
        } else {
            System.out.printf("호출 isOdd(%d)\n", num - 1);
        }
        return (num == 0) ? true : isOdd(num - 1);
    }

    public static void main(String[] args) {
        int num = 5;

        System.out.printf("호출 isEven(%d)\n", num);
        if (isEven(num)) {
            System.out.println(num + " is even");
        } else {
            System.out.println(num + " is odd");
        }
    }

}
```

```console
호출 isEven(5)
호출 isOdd(4)
호출 isEven(3)
호출 isOdd(2)
호출 isEven(1)
호출 isOdd(0)
return false
5 is odd
```

Tail Recursion

```java
package com.example.recursive2;

public class TailRecursion {
    /*
     * Single Recursion 로직은
     * 자식 메소드가 자신의 로직을 처리한 후 리턴한 값을 부모 메소드가 받아서 사용한다.
     * 
     * 반대로, 
     * Tail Recursion 로직은
     * 부모 메소드가 자신의 로직을 처리한 후 구한 값을 자식 메소드에게 전달하고 자식 메소드가 값을 받아서 사용한다.
     */
    public static int tailFactorial(int num, int... prev) {
        if (num < 0) {
            throw new IllegalArgumentException("Can't calculate factorial of negative");
        }

        // 메소드 사용 측에 편의를 위해서 두 번째 파라미터를 주지 않아도 되도록 코드적으로 처리한다.
        int sum = (prev.length > 0) ? (int) prev[0] : 1;

        if (num < 2) {
            System.out.println("return " + sum);
            return sum;
        } else {
            System.out.printf("tailFactorial(%d, %d)\n", num - 1, num * sum);
            return tailFactorial(num - 1, num * sum);
        }
    }

    public static void main(String[] args) {
        int num = 4;
        System.out.printf("호출 tailFactorial(%d)\n", num);
        System.out.printf("결과 %d! = %d\n\n", num, tailFactorial(num));

        /*
         * Tail Recursion 로직은 
         * 결과적으로 for문으로 처리하는 것과 같다.
         */
        System.out.println("반복문 시작");
        int sum = 1;
        for (int i = num; i > 0; i--) {
            System.out.printf("%d, %d\n", i, sum);
            sum *= i;
        }
        System.out.printf("결과 %d! = %d", num, sum);
    }

}
```

```console
호출 tailFactorial(4)
tailFactorial(3, 4)
tailFactorial(2, 12)
tailFactorial(1, 24)
return 24
결과 4! = 24

반복문 시작
4, 1
3, 4
2, 12
1, 24
결과 4! = 24
```

## 용례

Recursion Directory

```java
package com.example.recursive3;

public class RecursionExampleDirectory {

	public int getSize(Directory dir) {
		int total = 0;

		File[] files = dir.getFiles();
		for (int i = 0; i < files.length; i++) {
			// 파일객체의 getSize() 메소드는 바로 값을 리턴한다.
			total += files[i].getSize();
		}

		// get sub directories and check them
		Directory[] subDirs = dir.getDirectories();
		for (int i = 0; i < subDirs.length; i++) {
			// getSize() 메소드 호출은 재귀호출방식이다.
			total += getSize(subDirs[i]);
		}

		return total;
	}

	public static int pow(int base, int exp) {
		if (exp == 0) {
			System.out.println("return 1");
			return 1;
		} else {
			System.out.printf("return %d * pow(%d, %d)\n", base, base, exp - 1);
			return base * pow(base, exp - 1);
		}
	}

	public static void main(String[] args) {
		Directory directory = new Directory();

		RecursionExampleDirectory example = new RecursionExampleDirectory();
		System.out.println(example.getSize(directory));
		System.out.println();

		System.out.printf("호출 pow(%d, %d)\n", 2, 3);
		System.out.printf("%d의 %d제곱 = %d", 2, 3, RecursionExampleDirectory.pow(2, 3));
	}
}

class Directory {
	private Directory[] directories;
	private File[] files;

	public Directory() {
		int numberOfSubDirs = (int) (Math.random() * 2);
		System.out.println("numberOfSubDirs = " + numberOfSubDirs);
		directories = new Directory[numberOfSubDirs];

		int numberOfFiles = (int) (Math.random() * 3);
		System.out.println("numberOfFiles = " + numberOfFiles);
		files = new File[numberOfFiles];

		for (int i = 0; i < files.length; i++) {
			files[i] = new File(100);
		}

		for (int i = 0; i < directories.length; i++) {
			directories[i] = new Directory();
		}
	}

	public Directory[] getDirectories() {
		return directories;
	}

	public File[] getFiles() {
		return files;
	}
}

class File {
	private int size;

	public File(int size) {
		this.size = size;
	}

	public int getSize() {
		return size;
	}
}

```

```console
numberOfSubDirs = 1
numberOfFiles = 0
numberOfSubDirs = 1
numberOfFiles = 0
numberOfSubDirs = 1
numberOfFiles = 1
numberOfSubDirs = 0
numberOfFiles = 0
100

호출 pow(2, 3)
return 2 * pow(2, 2)
return 2 * pow(2, 1)
return 2 * pow(2, 0)
return 1
2의 3제곱 = 8
```



