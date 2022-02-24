# 카펫



###### 문제 설명

Leo는 카펫을 사러 갔다가 아래 그림과 같이 중앙에는 노란색으로 칠해져 있고 테두리 1줄은 갈색으로 칠해져 있는 격자 모양 카펫을 봤습니다.

![carpet.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/b1ebb809-f333-4df2-bc81-02682900dc2d/carpet.png)

Leo는 집으로 돌아와서 아까 본 카펫의 노란색과 갈색으로 색칠된 격자의 개수는 기억했지만, 전체 카펫의 크기는 기억하지 못했습니다.

Leo가 본 카펫에서 갈색 격자의 수 brown, 노란색 격자의 수 yellow가 매개변수로 주어질 때 카펫의 가로, 세로 크기를 순서대로 배열에 담아 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 갈색 격자의 수 brown은 8 이상 5,000 이하인 자연수입니다.
- 노란색 격자의 수 yellow는 1 이상 2,000,000 이하인 자연수입니다.
- 카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 깁니다.

##### 입출력 예

| brown | yellow | return |
| ----- | ------ | ------ |
| 10    | 2      | [4, 3] |
| 8     | 1      | [3, 3] |
| 24    | 24     | [8, 6] |

[출처](http://hsin.hr/coci/archive/2010_2011/contest4_tasks.pdf)

※ 공지 - 2020년 2월 3일 테스트케이스가 추가되었습니다.
※ 공지 - 2020년 5월 11일 웹접근성을 고려하여 빨간색을 노란색으로 수정하였습니다.





---------------





## 권지민

> (타일의 가로 * 세로) - (yellow 가로 * 세로) = brown 타일 개수.  
> 라는 공식을 이용한다.
>
> 1. yellow 수에 대한 약수를 구한다. (사각형이므로 yellow의 가로, 세로 후보를 구할 수 있다)
>
> 2. 구한 한 쌍의 약수에 각각 +2를 더한다. 해당 값은 각각 타일의 가로, 세로가 된다.
>
> 3. 위의 공식 기준으로 (타일의 가로 * 세로) - (yellow 가로 * 세로) = brown타일 개수가 성립하는지 확인한다.
>
> 4-1. 성립할 경우 해당 공식의 타일 가로, 세로값을 return.  
> 4-2. 성립하지 않을 경우 yellow의 다음 약수를 구해 2~3을 반복한다.

```java
import java.util.Arrays;
class Solution {
    public int[] solution(int brown, int yellow) {
        int[] answer = {0, 0};
        
        for (int i = 1; i * i <= yellow; i++) {
            if (yellow % i == 0) {
                answer[0] = yellow / i + 2; //가로가 세로보다 커야 하므로
                answer[1] = i + 2;
                if (answer[0] * answer[1] - i * yellow / i == brown)
                    return (answer);
            }
        }
        return answer;
    }
}
```





## 김성용

> 코드

```python

```





## 민성호

> yellow + brown = 가로 * 세로 이면서.  
> (가로-2)*(세로-2) = yellow 인 가로,세로 조합을 찾는다

```java
class Solution {
    public int[] solution(int brown, int yellow) {
        int[] ret = new int[2];
        int sum = brown+yellow;
        for (int i=3; i<=sum; i++) {
            for (int j=3; j<=sum/i; j++) {
                if (i*j == brown+yellow && (i-2)*(j-2)==yellow && i>=j) {
                    ret[0] = i;
                    ret[1] = j;
                    return ret;
                }
            }
        }
        return null;
    }
}
```
