## url
https://www.acmicpc.net/problem/1149

## 문제요약
1~N 까지의 집이 일렬로 존재하는데 각각의 R,G,B 색상으로 칠해야한다.
이웃간에는 같은 색깔로 칠하지 않고자 할때 최소비용을 찾는 문제이다

## 조건
**N까지의 누적 최소합 = N-1까지의 누적최소합 + N번째의 최소값**
둘중 어느 하나라도 최소값이 아닐경우 N번째까지의 누적 최소값이 최소값이라고 보장할 수 없다는 뜻입니다.

## 점화식 도출
1. i + 1번째 집까지의 최소 색칠 비용을 구한다 하면 경우의 수는 총 3가지가 나오게 된다.
2. - i+1번째 까지의 최소 색칠 비용 = R : min(i번째 G색상 까지의 최소 합, i번째 B색상 까지의 최소 합) + R 비용
   - i+1번째 까지의 최소 색칠 비용 = G : min(i번째 R색상 까지의 최소 합, i번째 B색상 까지의 최소 합) + G 비용
   - i+1번째 까지의 최소 색칠 비용 = B : min(i번째 R색상 까지의 최소 합, i번째 G색상 까지의 최소 합) + B 비용

## 소스

        public static final int R=0;
        public static final int G=1;
        public static final int B=2;

        public static void main(String[] args){
               Scanner sc = new Scanner(System.in);
	       int N = sc.nextInt();
	       if(N > 1000) return;
		
	       int[][] result = new int[3][N];
	       int r,g,b;
	       result[R][0] = sc.nextInt();
	       result[G][0] = sc.nextInt();
	       result[B][0] = sc.nextInt();
		
	       for(int i = 1 ; i < N ; i++){
	           r = sc.nextInt();
		   g = sc.nextInt();
		   b = sc.nextInt();
			
		   result[R][i] = r + Math.min(result[G][i-1],result[B][i-1]);
		   result[G][i] = g + Math.min(result[R][i-1],result[B][i-1]);
		   result[B][i] = b + Math.min(result[R][i-1],result[G][i-1]);
	        }
	   int min = Math.min(Math.min(result[R][N-1],result[G][N-1]),result[B][N-1]);
	   System.out.println(min);
	   sc.close();



