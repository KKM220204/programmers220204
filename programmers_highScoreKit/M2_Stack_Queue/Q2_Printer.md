# 프린터

###### 문제 설명

일반적인 프린터는 인쇄 요청이 들어온 순서대로 인쇄합니다. 그렇기 때문에 중요한 문서가 나중에 인쇄될 수 있습니다. 이런 문제를 보완하기 위해 중요도가 높은 문서를 먼저 인쇄하는 프린터를 개발했습니다. 이 새롭게 개발한 프린터는 아래와 같은 방식으로 인쇄 작업을 수행합니다.

```
1. 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.
2. 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.
3. 그렇지 않으면 J를 인쇄합니다.
```

예를 들어, 4개의 문서(A, B, C, D)가 순서대로 인쇄 대기목록에 있고 중요도가 2 1 3 2 라면 C D A B 순으로 인쇄하게 됩니다.

내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 알고 싶습니다. 위의 예에서 C는 1번째로, A는 3번째로 인쇄됩니다.

현재 대기목록에 있는 문서의 중요도가 순서대로 담긴 배열 priorities와 내가 인쇄를 요청한 문서가 현재 대기목록의 어떤 위치에 있는지를 알려주는 location이 매개변수로 주어질 때, 내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 현재 대기목록에는 1개 이상 100개 이하의 문서가 있습니다.
- 인쇄 작업의 중요도는 1~9로 표현하며 숫자가 클수록 중요하다는 뜻입니다.
- location은 0 이상 (현재 대기목록에 있는 작업 수 - 1) 이하의 값을 가지며 대기목록의 가장 앞에 있으면 0, 두 번째에 있으면 1로 표현합니다.

##### 입출력 예

| priorities         | location | return |
| ------------------ | -------- | ------ |
| [2, 1, 3, 2]       | 2        | 1      |
| [1, 1, 9, 1, 1, 1] | 0        | 5      |

##### 입출력 예 설명

예제 #1

문제에 나온 예와 같습니다.

예제 #2

6개의 문서(A, B, C, D, E, F)가 인쇄 대기목록에 있고 중요도가 1 1 9 1 1 1 이므로 C D E F A B 순으로 인쇄합니다.

[출처](http://www.csc.kth.se/contest/nwerc/2006/problems/nwerc06.pdf)



---------------

  

## 권지민

> 프린터.   
> 문서의 우선순위에 따라 큐 뒤에 배치할 일이 생기기 때문에 문서의 순서를 알기 위해.   
> 큐에는 문서의 index값으로 집어넣는다.
>
> 1. peek()함수를 통해 대기목록 가장 앞 문서를 꺼내온다.
> 2. findMax()함수를 이용하여 현재 문서가 가장 중요도가 높지 않다면 다시 해당 큐 마지막에 집어넣는다 (add)
> 3. 가장 중요도가 높을 경우 ArrayList에 집어넣는다.
>
> 해당 로직을 큐가 전부 비워질때까지 진행한 후, 반환값에 ArrayList값을 넣어 제출한다.

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
class Solution {
    public boolean findMax(Queue<Integer> queue, int[] p) {
        Queue<Integer> temp = new LinkedList<>();;
        temp.addAll(queue);
        int max = p[temp.poll()];
        while(temp.isEmpty() != true) {
            if (p[temp.peek()] > max) 
                return false;
            temp.poll();
        }
        return true;
    }
    public int solution(int[] priorities, int location) {
        int answer = 0;
        ArrayList<Integer> index = new ArrayList<>();
        Queue<Integer> queue = new LinkedList<>();
        
        for (int i = 0; i < priorities.length; i++) {
            queue.add(i);
        }
        
        while(queue.isEmpty() != true) {
            if (findMax(queue, priorities) == true)
                index.add(queue.poll());
            else {
                queue.add(queue.poll()); 
            }
        }
        for (int i = 0; i < index.size(); i++) {
            if (location == index.get(i))
                answer = i + 1;
        }
        return answer;
    }
}
```

  

## 김성용

> 대충 설명  
> 대충 설명2

```python

```

  

## 민성호

> 우선 순위와 인덱스를 각각 자료구조를 만들어 저장한다.   
> 가장 앞에 있는 자료의 우선 순위가 max이면 제거한다.  
> 아니라면 자료구조의 제일 뒤로 넣는다.  
> 제거하는 경우, 제거할 자료가 찾고 있는 자료이면 그 인덱스를 반환한다.  

```java
import java.util.*;

class Solution {
    public int solution(int[] priorities, int location) {
        Deque<Integer> idx = new ArrayDeque<>();
        Deque<Integer> prior = new ArrayDeque<>();

        for(int i=0; i<priorities.length; i++) {
            idx.addLast(i);
            prior.addLast(priorities[i]);
        }

        int cnt=0;
        while(true){
            if(isMax(prior,prior.peekFirst())){
                cnt++;
                int n=idx.getFirst();
                if(n==location) return cnt;
                idx.removeFirst();
                prior.removeFirst();
            } else {
                idx.addLast(idx.removeFirst());
                prior.addLast(prior.removeFirst());
            }
        }
    }

    public boolean isMax(Deque<Integer> dq, int val) {
        int max=0;
        for(Integer n : dq) {
            if (n > max) {
                max=n;
            }
        }
        return val==max;
    }
}
```

