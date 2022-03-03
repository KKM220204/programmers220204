# 정수 삼각형



###### 문제 설명

![스크린샷 2018-09-14 오후 5.44.19.png](https://grepp-programmers.s3.amazonaws.com/files/production/97ec02cc39/296a0863-a418-431d-9e8c-e57f7a9722ac.png)

위와 같은 삼각형의 꼭대기에서 바닥까지 이어지는 경로 중, 거쳐간 숫자의 합이 가장 큰 경우를 찾아보려고 합니다. 아래 칸으로 이동할 때는 대각선 방향으로 한 칸 오른쪽 또는 왼쪽으로만 이동 가능합니다. 예를 들어 3에서는 그 아래칸의 8 또는 1로만 이동이 가능합니다.

삼각형의 정보가 담긴 배열 triangle이 매개변수로 주어질 때, 거쳐간 숫자의 최댓값을 return 하도록 solution 함수를 완성하세요.

##### 제한사항

- 삼각형의 높이는 1 이상 500 이하입니다.
- 삼각형을 이루고 있는 숫자는 0 이상 9,999 이하의 정수입니다.

##### 입출력 예

| triangle                                                | result |
| ------------------------------------------------------- | ------ |
| [[7], [3, 8], [8, 1, 0], [2, 7, 4, 4], [4, 5, 2, 6, 5]] | 30     |

[출처](http://stats.ioinformatics.org/countries/SWE)





---------------





## 권지민

> 맨 아래 줄을 다 채워넣고, 재귀로 dp 실행
> 맨 밑의 층에 도달했을 때, 두 수를 비교해서 더 큰 수를 바로 위 수에 더한다. 이를 반복하여 맨 꼭대기에 최대 합이 정해진다.

```java
class Solution {
    int [][] dp;
    
    public int findMax(int index, int[][] triangle, int depth) {
        if (depth == triangle.length) 
            return dp[depth][index];
        if (dp[depth][index] == 0)
            dp[depth][index] = Math.max(findMax(index, triangle, depth + 1), //왼쪽 오른쪽 중 큰값 반환
                            findMax(index + 1, triangle, depth + 1)) + triangle[depth][index];
        return dp[depth][index];
    }
    public int solution(int[][] triangle) {
        dp = new int[triangle.length][triangle.length];
        //dp 맨 밑을 triangle과 똑같이 세팅
        for (int i = 0; i < triangle.length; i++)
            dp[triangle.length - 1][i] = triangle[triangle.length - 1][i];
        int max = findMax(0, triangle, 0);
        return max;
    }
}
```





## 김성용

> 코드

```python

```





## 민성호

> 밑에서부터 더한 값의 최대치들을 담는다

```java
class Solution {
    public int solution(int[][] triangle) {
        int len = triangle.length;
        
        int[] temp = new int[len];
        for(int i=0; i<len; i++) {
            temp[i] = triangle[len-1][i];
        }
        
        for (int i=len-1; i>-1; i--) {
            for (int j=0; j<i; j++) {
                temp[j] = Math.max(triangle[i-1][j]+temp[j], triangle[i-1][j]+temp[j+1]);
            }
        }
        return temp[0];
    }
}
```
