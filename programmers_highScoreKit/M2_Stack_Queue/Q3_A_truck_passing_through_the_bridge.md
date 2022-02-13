# 다리를 지나는 트럭

###### 문제 설명

트럭 여러 대가 강을 가로지르는 일차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며, 다리는 weight 이하까지의 무게를 견딜 수 있습니다. 단, 다리에 완전히 오르지 않은 트럭의 무게는 무시합니다.

예를 들어, 트럭 2대가 올라갈 수 있고 무게를 10kg까지 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

| 경과 시간 | 다리를 지난 트럭 | 다리를 건너는 트럭 | 대기 트럭 |
| --------- | ---------------- | ------------------ | --------- |
| 0         | []               | []                 | [7,4,5,6] |
| 1~2       | []               | [7]                | [4,5,6]   |
| 3         | [7]              | [4]                | [5,6]     |
| 4         | [7]              | [4,5]              | [6]       |
| 5         | [7,4]            | [5]                | [6]       |
| 6~7       | [7,4,5]          | [6]                | []        |
| 8         | [7,4,5,6]        | []                 | []        |

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리에 올라갈 수 있는 트럭 수 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭 별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

##### 제한 조건

- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.

##### 입출력 예

| bridge_length | weight | truck_weights                   | return |
| ------------- | ------ | ------------------------------- | ------ |
| 2             | 10     | [7,4,5,6]                       | 8      |
| 100           | 100    | [10]                            | 101    |
| 100           | 100    | [10,10,10,10,10,10,10,10,10,10] | 110    |

[출처](http://icpckorea.org/2016/ONLINE/problem.pdf)

※ 공지 - 2020년 4월 06일 테스트케이스가 추가되었습니다.



---------------

  

## 권지민

> 다리를 지나는 트럭.   
> 다리를 건너는 중인 트럭을 큐에 넣는다. 단, 큐에 삽입 전 다리의 무게를 넘지 않도록 고려한다.  
> 큐에 들어가는 인자들은 [트럭이 다리를 다 건널때의 시간] = [현재 초 + 다리의 길이] 를 집어넣는다.  
> 트럭이 다리를 다 건너면, 큐에서 해당 트럭을 빼고 대기중인 트럭을 넣을지 여부를 결정한다.  
> 트럭이 모두 다리를 다 건너면 끝

```java
import java.util.LinkedList;
import java.util.Queue;
class Solution {
    class truck {
        int time;
        int weight;
        
        public truck(int time, int weight) {
            this.time = time;
            this.weight = weight;
        }
    }
    public int solution(int bridge_length, int weight, int[] truck_weights) {
        int answer = 2;
        int totalWeight = truck_weights[0];
        Queue<truck> wait = new LinkedList<>();
        Queue<truck> running = new LinkedList<>();
        
        for(int truck : truck_weights)
            wait.add(new truck(bridge_length, truck));
        wait.poll();
        running.add(new truck(bridge_length + 1, truck_weights[0]));
        
        while(!(running.isEmpty() && wait.isEmpty())) {
            if (running.peek().time == answer) {
                totalWeight -= running.peek().weight;
                running.poll();
            }
            if (!wait.isEmpty() && totalWeight + wait.peek().weight <= weight) { 
                running.add(new truck(answer + wait.peek().time, wait.peek().weight));
                totalWeight += wait.poll().weight;
            }
            if (running.isEmpty() && wait.isEmpty())
                break;
            answer++;
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

> 다리(다리 길이만큼, list)와, 대기 목록을 시뮬레이션 할 자료 구조를 만든다.  
> 날짜 계산 변수(cnt)를 하나 줘서 날짜가 하나씩 증가할 때 마다.  
> 다리의 list 변수들을 왼쪽으로 한 칸씩 밀어 넣는다.  
> 밀어 넣을 때, 다음 트럭이 올 수 있으면 다음 트럭을 밀어 넣고, 아니면 0을 넣는다.  
> list, 대기 목록의 멤버 합이 0이 될 때 까지 반복하고 게산된 날짜를 반환한다.

```java
import java.util.*;

class Solution {
    public int solution(int bridge_length, int weight, int[] truck_weights) {
        Deque<Integer> moving = new ArrayDeque<>();
        Deque<Integer> waiting = new ArrayDeque<>();
            
        for(int i=0; i<bridge_length; i++){
            moving.addLast(0);
        }
        for(int i=0; i<truck_weights.length; i++){
            waiting.addLast(truck_weights[i]);
        }

        int day=0;
        while (!(sumOfDq(moving)==0 && sumOfDq(waiting)==0) ) {
            moving.removeFirst();
            if(sumOfDq(moving) + waiting.getFirst() <= weight){
                moving.addLast(waiting.removeFirst());
                waiting.addLast(0);
            } else {
                moving.addLast(0);
            }
            day++;
        }
        return day;
    }

    public int sumOfDq(Deque<Integer> dq){
        int sum=0;
        for(Integer n : dq) {
            sum+=n;
        }
        return sum;
    }
}
```

