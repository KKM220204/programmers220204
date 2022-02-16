# 더 맵게



###### 문제 설명

이중 우선순위 큐는 다음 연산을 할 수 있는 자료구조를 말합니다.

| 명령어 | 수신 탑(높이)                  |
| ------ | ------------------------------ |
| I 숫자 | 큐에 주어진 숫자를 삽입합니다. |
| D 1    | 큐에서 최댓값을 삭제합니다.    |
| D -1   | 큐에서 최솟값을 삭제합니다.    |

이중 우선순위 큐가 할 연산 operations가 매개변수로 주어질 때, 모든 연산을 처리한 후 큐가 비어있으면 [0,0] 비어있지 않으면 [최댓값, 최솟값]을 return 하도록 solution 함수를 구현해주세요.

##### 제한사항

- operations는 길이가 1 이상 1,000,000 이하인 문자열 배열입니다.
- operations의 원소는 큐가 수행할 연산을 나타냅니다.
  - 원소는 “명령어 데이터” 형식으로 주어집니다.- 최댓값/최솟값을 삭제하는 연산에서 최댓값/최솟값이 둘 이상인 경우, 하나만 삭제합니다.
- 빈 큐에 데이터를 삭제하라는 연산이 주어질 경우, 해당 연산은 무시합니다.

##### 입출력 예

| operations                  | return |
| --------------------------- | ------ |
| ["I 16","D 1"]              | [0,0]  |
| ["I 7","I 5","I -5","D -1"] | [7,5]  |

##### 입출력 예 설명

16을 삽입 후 최댓값을 삭제합니다. 비어있으므로 [0,0]을 반환합니다.
7,5,-5를 삽입 후 최솟값을 삭제합니다. 최대값 7, 최소값 5를 반환합니다.

[출처](http://icpckorea.org/problems/2013/onlineset.pdf)



---------------





## 권지민

> 최대값 우선순위, 최소값 우선순위큐를 두개 만든다. 또한 실제 큐의 size 변수를 하나 선언한다.  
> 명령어를 파싱하여 해당되는 명령을 수행한다.
>
> 1. 숫자 삽입 : 큐 두개에 해당 숫자를 넣는다
> 2. 최대값 삭제 : 최대값 큐에서 원소를 지운다.
> 3. 최소값 삭제 : 최소값 큐에서 원소를 지운다.  
> 다만, 실제 size가 0이 될 경우는 양쪽 큐 안의 원소를 전부 다 초기화 시켜준다.  
> 큐의 원소가 없는 경우 초기화된 변수 [0,0]리턴.  
> 원소가 2개면 최소, 최대값을 각각 answer에 넣어 반환한다.

```java
import java.util.PriorityQueue;
import java.util.Collections;
class Solution {
    public void erase(PriorityQueue<Integer> num, PriorityQueue<Integer> num2, int size) {
        num.poll();
        if (size == 0) {
            while (num.size() != 0)
                num.poll();
            while (num2.size() != 0)
                num2.poll();
        }
    }
    public int[] solution(String[] operations) {
        int[] answer = {0, 0};
        PriorityQueue<Integer> max = new PriorityQueue<>(Collections.reverseOrder()); 
        PriorityQueue<Integer> min = new PriorityQueue<>();
        int size = 0;
        
        for (int i = 0; i < operations.length; i++) {
            if (operations[i].contains("I")) {
                String num = operations[i].substring(2);
                size++;
                max.add(Integer.parseInt(num));
                min.add(Integer.parseInt(num));
            }
            else if (operations[i].contains("-") && min.size() != 0) {
                size--;
                erase(min, max, size);
            }
            else {
                if (max.size() != 0) {
                    size--;
                }
                erase(max, min, size);
            }
        }
        if (size >= 2) {
            answer[0] = max.peek();
            answer[1] = min.peek();
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

> 오름차순, 내림차순으로 정렬될 우선순위 큐를 각각 만든다 (총 2개).  
> 파싱해서 값을 추가하는 경우('I') 두 큐 모두에 값을 넣어준다.  
> 최대값을 제거하는 경우 내림차순큐(최대힙)에서 poll메서드를 실행한다.  
> 최소값을 제거하는 경우 오름차순큐(최소힙)에서 poll메서드를 실행한다.  
> size가 0이면 0을, 사이즈가 있으면 최대,최소값을 반환한다

```java
import java.util.*;

class Solution {
    public int[] solution(String[] operations) {
        PriorityQueue<Integer> listAsc = new PriorityQueue<>();
        PriorityQueue<Integer> listDes = new PriorityQueue<>(Collections.reverseOrder());
        
        for (String str : operations) {
            String[] temp = str.split(" ");
            String operator = temp[0];
            int num = Integer.parseInt(temp[1]);
            
            if (operator.equals("I")) {
                listAsc.add(num);
                listDes.add(num);
            }
            if (listAsc.size() > 0 && listDes.size() > 0 && operator.equals("D")) {
                if (num == 1) {
                    listAsc.remove(listDes.poll());
                } else if (num == -1) {
                    listDes.remove(listAsc.poll());
                }
            }
        }
        int[] ret = new int[2];
        ret[0] = listDes.size()==0 ? 0 : listDes.peek();
        ret[1] = listAsc.size()==0 ? 0 : listAsc.peek();
        return ret;
    }
}
```
