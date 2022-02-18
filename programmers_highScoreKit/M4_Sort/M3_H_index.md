# H-Index



- H-Index
- darklight

- sublimevimemacs

- Java 

###### 문제 설명

H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과[1](https://programmers.co.kr/learn/courses/30/lessons/42747#fn1)에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 `n`편 중, `h`번 이상 인용된 논문이 `h`편 이상이고 나머지 논문이 h번 이하 인용되었다면 `h`의 최댓값이 이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
- 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.

##### 입출력 예

| citations       | return |
| --------------- | ------ |
| [3, 0, 6, 1, 5] | 3      |

##### 입출력 예 설명

이 과학자가 발표한 논문의 수는 5편이고, 그중 3편의 논문은 3회 이상 인용되었습니다. 그리고 나머지 2편의 논문은 3회 이하 인용되었기 때문에 이 과학자의 H-Index는 3입니다.

※ 공지 - 2019년 2월 28일 테스트 케이스가 추가되었습니다.

------

1. https://en.wikipedia.org/wiki/H-index "위키백과" [↩](https://programmers.co.kr/learn/courses/30/lessons/42747#fnref1)





---------------





## 권지민

> 1. 논문 인용횟수 배열 citations를 정렬한다.
> 2. 오름차순이므로 가장 큰 수인 맨 뒤 index부터 하나씩 answer와 세어가며 answer값을 올려준다.
> 3. answer이 논문 인용횟수보다 크다면 해당 answer값을 H-index로 반환한다.

```java
import java.util.Arrays;
import java.util.Collections;
class Solution {
    public int solution(int[] citations) {
        int answer = 0;
        
        Arrays.sort(citations);
        for (int i = citations.length - 1; i > -1; i--) {
            if (answer < citations[i] )
                answer++;
            else
                break;
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

> 오름차순으로 정렬한다.   
> 배열을 순회하면서, h인덱스 값의 최대치가 될 수 있는 배열의 길이에서 시작.  
> 배열의 요소가 h값보다 작으면 h값을 줄인다

```java
import java.util.*;

class Solution {
    public int solution(int[] citations) {
        Arrays.sort(citations);
        int h = citations.length;
        
        for(int i=0; i<citations.length; i++) {
            if (citations[i] < h) {
                h--;
            } else {
                break;
            }
        }
        return h;
    }
}
```
