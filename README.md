# Karatsuba-For-Multiplication-with-Divide-and-Conquer
---

The Karatsuba algorithm is a fast multiplication algorithm that was discovered by Anatolii Alexeevitch Karatsuba in 1960. It is a divide-and-conquer algorithm designed to multiply two numbers efficiently by breaking down the problem into smaller sub-problems.

Let's consider two n-digit numbers, A and B, where n is a positive integer. The goal is to compute their product, C = A * B.

The Karatsuba algorithm follows these steps:

1. Split the input numbers:
   * Split the input numbers A and B each into two halves, forming A = A1 * 10^(n/2) + A0 and B = B1 * 10^(n/2) + B0, where A1 and B1 are the most significant halves, and A0 and B0 are the least significant halves.

2. Recursive multiplication:
   * P1 = A1 * B1
   * P2 = A0 * B0
   * P3 = (A1 + A0) * (B1 + B0)
3. Combine the products:
   * Calculate the intermediate result Z1 = P3 - P1 - P2.

4. Final result construction:
   * The final result C is given by the formula:
     C = P1 * 10^n + P2 + Z1 * 10^(n/2)

This algorithm relies on the observation that the product of two numbers can be expressed in terms of three multiplications instead of the traditional four. The recursive nature of the algorithm helps reduce the overall number of multiplications.

The time complexity of the Karatsuba algorithm is O(n^log₂(3)), which is better than the traditional multiplication algorithm's O(n^2) for large enough numbers.

Here's a simple example:

Let A = 1234 and B = 5678. Then, we split them into A1 = 12, A0 = 34 and B1 = 56, B0 = 78.

P1 = 12 * 56 = 672
P2 = 34 * 78 = 2652
P3 = (12 + 34) * (56 + 78) = 46 * 134 = 6164
Z1 = P3 - P1 - P2 = 6164 - 672 - 2652 = 2840
Now, construct the final result using the formula:
C = P1 * 10000 + P2 + Z1 * 100 = 6720000 + 2652 + 284000 = 7006652

## Explan the code 
This Java program implements the Karatsuba algorithm for multiplying two large integers. Here's a description of the code:

1. **Main Class**:
   * The program starts by declaring a boolean variable `flag` and initializing it to `false`.
   * It enters a do-while loop, where it attempts to execute the following block of code.
   * Inside the loop:
     * A Scanner object (`sc`) is created to take input from the user.
     * The user is prompted to enter two long integers (`First` and `Second`).
     * The `KaratsubaForMultiplication` method is called with the entered values, and the result is printed.
     * If an exception occurs (e.g., if the user enters a non-integer value), an error message is displayed, and the `flag` is set to `true` to repeat the loop.
     * The loop continues until valid input is provided.

2. **KaratsubaForMultiplication Method**:

   * This method takes two long integers (`First` and `Second`) as input and returns their product using the Karatsuba algorithm.
   * It calculates the sizes of the input numbers using the `GetSize` method.
   * If the size of the numbers is less than 10, it performs a simple multiplication and returns the result.
   * Otherwise, it recursively divides the numbers into smaller parts and applies the Karatsuba algorithm.
   * The final result is computed using the recursive results and appropriate powers of 10.

3. **GetSize Method**:

   * This method calculates the number of digits in a given long integer (`A`).
   * It iteratively divides the number by 10 and increments the size until the number becomes zero.
   * 
The overall purpose of the program is to demonstrate the Karatsuba algorithm for efficient multiplication of large integers. It continuously prompts the user for input until valid integer values are provided and then displays the result of the multiplication using the Karatsuba algorithm.

## Code 
---

    import java.util.Scanner;

    public class Main {
    public static void main(String[] args) {
        boolean flag = false;
        do {
            try {
                Scanner sc = new Scanner(System.in);
                System.out.print("Enter First Number : ");
                long First = sc.nextLong();
                System.out.print("\nEnter Second Number : ");
                long Second = sc.nextLong();
                System.out.println("Result of Multiplication of " + First + " And " + Second + " by using Karatsuba Algorithm");
                System.out.println("First X Second = " + KaratsubaForMultiplication(First, Second));
                System.exit(0);
            } catch (Exception e) {
                System.out.println("Enter Integer Value ");
                System.out.println(" Try Again ");
                flag = true;
            }
        } while (flag);
    }

    public static long KaratsubaForMultiplication(long First, long Second) {
        int SizeFirst = GetSize(First);
        int SizeSecond = GetSize(Second);
        int Length = Math.max(SizeFirst, SizeSecond);
        if (Length < 10) {
            return First * Second;
        }
        Length = (Length / 2) + (Length % 2);
        long M = (long) Math.pow(10, Length);
        /*----Divide Each Number to Two Number----*/
        long A = First / M;
        long B = First - (M * A);
        long C = Second / M;
        long D = Second - (M * C);
        long Z0 = KaratsubaForMultiplication(A, C);
        long Z2 = KaratsubaForMultiplication(B, D);
        long Z1 = KaratsubaForMultiplication(A + B, C + D) - Z0 - Z2;
        return Z0 * (long) Math.pow(10, Length) + Z1 * (long) Math.pow(10, (Length / 2)) + Z2;
    }

    public static int GetSize(long A) {
        int size = 0;
        while (A != 0) {
            A /= 10;
            size++;
        }
        return size;
    }
    }
