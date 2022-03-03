# 등굣길



###### 문제 설명

계속되는 폭우로 일부 지역이 물에 잠겼습니다. 물에 잠기지 않은 지역을 통해 학교를 가려고 합니다. 집에서 학교까지 가는 길은 m x n 크기의 격자모양으로 나타낼 수 있습니다.

아래 그림은 m = 4, n = 3 인 경우입니다.

![image0.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/056f54e618/f167a3bc-e140-4fa8-a8f8-326a99e0f567.png)

가장 왼쪽 위, 즉 집이 있는 곳의 좌표는 (1, 1)로 나타내고 가장 오른쪽 아래, 즉 학교가 있는 곳의 좌표는 (m, n)으로 나타냅니다.

격자의 크기 m, n과 물이 잠긴 지역의 좌표를 담은 2차원 배열 puddles이 매개변수로 주어집니다. **오른쪽과 아래쪽으로만 움직여** 집에서 학교까지 갈 수 있는 최단경로의 개수를 1,000,000,007로 나눈 나머지를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 격자의 크기 m, n은 1 이상 100 이하인 자연수입니다.
  - m과 n이 모두 1인 경우는 입력으로 주어지지 않습니다.
- 물에 잠긴 지역은 0개 이상 10개 이하입니다.
- 집과 학교가 물에 잠긴 경우는 입력으로 주어지지 않습니다.

##### 입출력 예

| m    | n    | puddles  | return |
| ---- | ---- | -------- | ------ |
| 4    | 3    | [[2, 2]] | 4      |

##### 입출력 예 설명

![image1.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/32c67958d5/729216f3-f305-4ad1-b3b0-04c2ba0b379a.png)





---------------





## 권지민

> 가는 경우의 수 : 오른쪽 한칸, 아래 한칸.  
> 현재 내 타일 (1, 0)일 때, 바로 전에서 올 수 있는 경로는 (0,0) 하나뿐. dp[1][0] = 1.  
> (1, 2)일 때, 이전 (1,1)과 (0,2)에서 올수 있는 경로를 더한다. dp[1][2] = dp[1][1] + dp[0][2];   
> 다만 웅덩이인 경우는 -1 넣고 제외

```java
class Solution {
    public int solution(int m, int n, int[][] puddles) {
        int answer = 0;
        int [][]dp = new int[n][m];
        
        for (int[] i : puddles)
            dp[i[1] - 1][i[0] - 1] = -1;
        dp[0][0] = 1;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (dp[i][j] == 0) {
                    if (i != 0 && dp[i - 1][j] != -1)
                        dp[i][j] += dp[i - 1][j];
                    if (j != 0 && dp[i][j - 1] != -1)
                        dp[i][j] += dp[i][j - 1];
                    dp[i][j] %= 1000000007;
                }
            }
        }
        return dp[n - 1][m - 1];
    }
}
```





## 김성용

> 코드

```python

```





## 민성호

> 목표지점에서부터 시작지점까지 더해나간다

```java
class Solution {
    int rowNum;
    int colNum;
    int[][] path;
    
    public int solution(int m, int n, int[][] puddles) {
        rowNum = n;
        colNum = m;
        path = new int[m][n];
        path[m-1][n-1] = 1;
        
        for (int i=m-1; i>=0; i--) {
            for(int j=n-1; j>=0; j--) {
                if (isInsidePath(i+1,j) && !isPond(i+1,j,puddles)) {
                    path[i][j] = (path[i][j]+path[i+1][j])%1000000007;
                }
                if (isInsidePath(i,j+1) && !isPond(i,j+1,puddles)) {
                    path[i][j] = (path[i][j]+path[i][j+1])%1000000007;                    
                }
            }
        }
        
        return path[0][0];
    }
    
    public boolean isInsidePath(int i, int j) {
        if (i > colNum-1 || j > rowNum-1) {
            return false;
        }
        return true;
    }
    
    public boolean isPond(int i, int j, int[][] pond) {
        for (int[] p : pond) {
            if(p[0]-1==i && p[1]-1==j) {
                return true;
            }
        }
        return false;
    }
    
}
```
