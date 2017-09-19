
## url
https://www.acmicpc.net/problem/1912

## 문제요약
n개의 정수로 이루어진 임의의 수열이 주어진다. 
우리는 이 중 연속된 몇 개의 숫자를 선택해서 구할 수 있는 합 중 가장 큰 합을 구하려고 한다.
예를 들어서 10, -4, 3, 1, 5, 6, -35, 12, 21, -1 이라는 수열이 주어졌다고 하자. 여기서 정답은 12+21인 33이 정답이 된다.

## 조건
숫자는 한 개 이상 선택해야 한다.
첫째 줄에 정수 n(1≤n≤100,000)이 주어지고 둘째 줄에는 n개의 정수로 이루어진 수열이 주어진다. 수는 -1,000보다 크거나 같고, 1,000보다 작거나 같은 정수이다.

## 점화식 도출
현재 index의 숫자와
지난 index까지의 최대합을 계속해서 비교해 나가는 방식이다.
예제.
10, -4, 3, 1, 5, 6, -35, 12, 21, -1
numbers     10 -4  3  1  5  6 -35 12 21 -1
sum_numbers 10  6  9 10 15 21 -14 12 33 32

sum_numbers[0] = max(0,numbers[0])
sum_numbers[1] = max(sum_numbers[0]+numbers[1],numbers[1])
sum_numbers[2] = max(sum_numbers[1]+numbers[2],numbers[2])
...


## 소스

	import java.util.Scanner;
	import java.util.stream.IntStream;

	public class Main {
	    public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int length = Integer.parseInt(sc.nextLine());
		int[] numbers = new int[length];
		int[] sum_numbers = new int[length];
		String[] nums = sc.nextLine().split(" ");
		for(int i = 0 ; i < length ; i++){
		    numbers[i] = Integer.parseInt(nums[i]);
		}

		sum_numbers[0] = numbers[0];
		int temp = 0;
		for (int i = 1; i < length; i++) {
		    temp = sum_numbers[i - 1] + numbers[i];
		    sum_numbers[i] = Math.max(temp, numbers[i]);
		}

		IntStream.of(sum_numbers).max().ifPresent(maxInt -> System.out.println(maxInt));
		sc.close();
	    }
	}



