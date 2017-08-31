## url
https://www.acmicpc.net/problem/11726

## 문제요약
2×n 크기의 직사각형을 1×2, 2×1 타일로 채우는 방법의 수

## 조건
첫째 줄에 2×n 크기의 직사각형을 채우는 방법의 수를 10,007로 나눈 나머지를 출력한다.

## 점화식 도출
1. 예를 하나 뽑아서 2 x 6 크기의 공간을 채우는 경우를 생각해 본다.
   여기서 2 x 6 크기의 공간을 채우는 여러 경우들을 2가지로 나눌 수 있다.
   
2. 남은 부분의 크기를 보면,A의 경우 나머지 공간의 너비는 5이고 B의 나머지 공간의 너비는 4이다.

3. A에서 남은 부분을 채우는 방법의 수는, 이 문제에서 너비가 5인 공간을 채우는 경우의 답과 같다.
   B의 나머지 부분을 채우는 방법의 수 역시 너비가 4인 공간을 채우는 경우의 답과 같다.
   여기서 이 문제의 재귀적 속성을 발견할 수 있으며
   이 문제를 Dynamic Programing (동적 계획법)으로 풀 수 있다는 것을 알 수 있다.

4. 상향식(bottom-up)으로 어떻게 풀 지를 구체적으로 생각해 보면,
   - n=1 일 때 (1 가지)
   - n=2 일 때 (2 가지)
   - n=3 일 때는  (n=2의 가지수) + (n=1의 가지수)
   
   'n=2의 가지수'는 위 그림에서 A의 경우이고, 'n=1의 가지수'는 위 그림에서 B이다.
   - n=4일 때는 (n=3)의 가지수 + (n=2의 가지수)
   n = k일 때는 (n=k-1 의 가지수) + (n=k-2 의 가지수) 
   어디서 본 적이 있는 점화식이 나온다. 바로 피보나치 수열이다.


## 소스

	import java.util.*;
	
	class Main 
	{
		static int[] dp = new int[10001];

		public static void main(String args[]) {
			Scanner sc = new Scanner(System.in);
			int n = sc.nextInt();
			System.out.println(f(n));
		}

		static public int f(int n)
		{
			if(n==0 || n==1) return 1;
			if(dp[n] > 0) return dp[n];
			dp[n] = (f(n-1) + f(n-2))%10007;
			return dp[n];
		}
	}

