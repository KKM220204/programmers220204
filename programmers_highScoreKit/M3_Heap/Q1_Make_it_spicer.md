# 더 맵게

###### 문제 설명

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

```
섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
```

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

##### 제한 사항

- scoville의 길이는 2 이상 1,000,000 이하입니다.
- K는 0 이상 1,000,000,000 이하입니다.
- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

##### 입출력 예

| scoville             | K    | return |
| -------------------- | ---- | ------ |
| [1, 2, 3, 9, 10, 12] | 7    | 2      |

##### 입출력 예 설명

1. 스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
   새로운 음식의 스코빌 지수 = 1 + (2 * 2) = 5
   가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]
2. 스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
   새로운 음식의 스코빌 지수 = 3 + (5 * 2) = 13
   가진 음식의 스코빌 지수 = [13, 9, 10, 12]

모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.



---------------





## 권지민

> 우선순위 큐에 스코빌 지수가 담긴 음식들을 넣으면, 루트노드 : 가장 맵지 않은 음식의 스코빌 지수 가 된다.  
> 따라서 큐에서 값을 두개 꺼내면 해당 공식의 인자를 구할 수 있음.  
> 두 음식을 섞어 다시 큐에 집어 넣으며 루트 노드가 K이상일 경우 or 더는 섞을 음식이 없을 경우까지 반복한다.

```java
import java.util.PriorityQueue;
class Solution {
    public int solution(int[] scoville, int K) {
        int answer = 0;
        PriorityQueue<Integer> que = new PriorityQueue<>();
        
        for (int i = 0; i < scoville.length; i++) {
            que.add(scoville[i]);
        }
        while(que.peek() < K) {
            if (que.size() == 1)
                return -1;
            que.add(que.poll() + que.poll() * 2);
            answer++;
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

> 오름차순으로 정렬되는 우선순위 큐에 주어진 스코빌 지수들을 담는다.  
> 2개를 뽑아서 공식에 따라 재조합한 후, 다시 우선순위 큐에 넣는다.  
> 큐의 첫 번째 요소가 기준 이상이면 조합 횟수를 리턴한다. 

```java
import java.util.*;

class Solution {
    public int solution(int[] scoville, int K) {
        
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int n : scoville) {
            pq.add(n);
        }
        int n;
        int cnt=0;
        while (pq.size()>1){
            pq.add(pq.poll() + pq.poll()*2);
            cnt++;
            if (pq.peek()>=K) return cnt;
        }
        return -1;
    }
}
```
