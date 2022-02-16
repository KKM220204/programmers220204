# 디스크 컨트롤러



###### 문제 설명

하드디스크는 한 번에 하나의 작업만 수행할 수 있습니다. 디스크 컨트롤러를 구현하는 방법은 여러 가지가 있습니다. 가장 일반적인 방법은 요청이 들어온 순서대로 처리하는 것입니다.

예를들어

```
- 0ms 시점에 3ms가 소요되는 A작업 요청
- 1ms 시점에 9ms가 소요되는 B작업 요청
- 2ms 시점에 6ms가 소요되는 C작업 요청
```

와 같은 요청이 들어왔습니다. 이를 그림으로 표현하면 아래와 같습니다.
![Screen Shot 2018-09-13 at 6.34.58 PM.png](https://grepp-programmers.s3.amazonaws.com/files/production/b68eb5cec6/38dc6a53-2d21-4c72-90ac-f059729c51d5.png)

한 번에 하나의 요청만을 수행할 수 있기 때문에 각각의 작업을 요청받은 순서대로 처리하면 다음과 같이 처리 됩니다.
![Screen Shot 2018-09-13 at 6.38.52 PM.png](https://grepp-programmers.s3.amazonaws.com/files/production/5e677b4646/90b91fde-cac4-42c1-98b8-8f8431c52dcf.png)

```
- A: 3ms 시점에 작업 완료 (요청에서 종료까지 : 3ms)
- B: 1ms부터 대기하다가, 3ms 시점에 작업을 시작해서 12ms 시점에 작업 완료(요청에서 종료까지 : 11ms)
- C: 2ms부터 대기하다가, 12ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 16ms)
```

이 때 각 작업의 요청부터 종료까지 걸린 시간의 평균은 10ms(= (3 + 11 + 16) / 3)가 됩니다.

하지만 A → C → B 순서대로 처리하면
![Screen Shot 2018-09-13 at 6.41.42 PM.png](https://grepp-programmers.s3.amazonaws.com/files/production/9eb7c5a6f1/a6cff04d-86bb-4b5b-98bf-6359158940ac.png)

```
- A: 3ms 시점에 작업 완료(요청에서 종료까지 : 3ms)
- C: 2ms부터 대기하다가, 3ms 시점에 작업을 시작해서 9ms 시점에 작업 완료(요청에서 종료까지 : 7ms)
- B: 1ms부터 대기하다가, 9ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 17ms)
```

이렇게 A → C → B의 순서로 처리하면 각 작업의 요청부터 종료까지 걸린 시간의 평균은 9ms(= (3 + 7 + 17) / 3)가 됩니다.

각 작업에 대해 [작업이 요청되는 시점, 작업의 소요시간]을 담은 2차원 배열 jobs가 매개변수로 주어질 때, 작업의 요청부터 종료까지 걸린 시간의 평균을 가장 줄이는 방법으로 처리하면 평균이 얼마가 되는지 return 하도록 solution 함수를 작성해주세요. (단, 소수점 이하의 수는 버립니다)

##### 제한 사항

- jobs의 길이는 1 이상 500 이하입니다.
- jobs의 각 행은 하나의 작업에 대한 [작업이 요청되는 시점, 작업의 소요시간] 입니다.
- 각 작업에 대해 작업이 요청되는 시간은 0 이상 1,000 이하입니다.
- 각 작업에 대해 작업의 소요시간은 1 이상 1,000 이하입니다.
- 하드디스크가 작업을 수행하고 있지 않을 때에는 먼저 요청이 들어온 작업부터 처리합니다.

##### 입출력 예

| jobs                     | return |
| ------------------------ | ------ |
| [[0, 3], [1, 9], [2, 6]] | 9      |

##### 입출력 예 설명

문제에 주어진 예와 같습니다.

- 0ms 시점에 3ms 걸리는 작업 요청이 들어옵니다.
- 1ms 시점에 9ms 걸리는 작업 요청이 들어옵니다.
- 2ms 시점에 6ms 걸리는 작업 요청이 들어옵니다.





---------------





## 권지민

> 1. 들어온 jobs를 작업 요청 시점순으로 정렬한다. (만약 시점이 같은 변수가 여러개 있다면, 직업 소요시간이 적은 것을 우선순위로 둔다.)
>
> 2. 해당 jobs를 우선순위큐에 담는다. 기준은 작업 소요시간이 적은 것(같으면 요청 시간이 빠른 순), 타입은 직접 정의한 객체 [Job] 
> 3. 큐에 저장된 값을 뺀다.(가장 소요시간이 빠른 작업이 나오게 된다.) 
>      3-1. 작업 요청 시간이 현재 시간보다 늦을 경우 (작업을 바로 진행할 수 없는 경우)
>
>             해당 작업을 temp에 저장해두고, 큐에서 다음 루트 노드를 꺼내 작업 요청 시간과 현재 시간을 비교해서 유효한 시간이 나올때까지              작업을 계속 뽑는다. (유효하지 않은 작업은 계속 temp 우선순위 큐에 넣어둔다)
>          
>             유효한 작업이 나오면 총 걸린 작업시간을 answer 에 합산해준다.
>             [총 걸린 작업시간] : [작업 소요시간] + [현재 시간 - 요청 시간 (요청된 시점 이후로 대기한 시간)]
>             temp에 빼놨던 작업들을 다시 기존 큐에 집어넣는다.
>
>      3-2. 작업 요청 시간이 현재 시간보다 이전일 경우 (작업을 바로 진행할 수 있음)
>
>             총 걸린 작업시간을 answer 에 합산해준다.
>             [총 걸린 작업시간] : [작업 소요시간] + [현재 시간 - 요청 시간 (요청된 시점 이후로 대기한 시간)]
>
> 4. 현재 시간을 1초 올려준다. (3-1에서 모든 작업 요청 시간이 현재 시간보다 늦을 경우, 아무런 일 없이 4번으로 넘어간다.)
>
> 5. 모든 작업이 수행 될 때까지 3번, 4번을 반복한 후, 평균 작업시간을 반환한다.
>     [평균 작업시간] : 작업시간 합산된 answer / 총 작업 수

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

> 주어진 값(int배열)을 요청 시간 기준, 작업 소요 시간 기준으로 정렬 되는 우선순위큐를 각각 만든다 (총 2개)
> 각각 이름은 wating큐, ready큐라고 한다. Waiting 큐에 입력값을 모두 담는다
> 작업 요청 시간이, 현재 시간보다 작거나 같은 경우는 레디큐에 넣는다 (없으면 현재 시간에 1을 더)
> 레디큐를 돌면서 작업한 시간과 현재 시간을 갱신하고, 레디큐에서 뺀다.

```java
import java.util.*;

class Solution {
    public int solution(int[][] jobs) {
        PriorityQueue<int[]> waiting = new PriorityQueue<>(new Comparator<int[]>(){
            public int compare(int[] a, int[] b) {
                return a[0]-b[0];
            }
        });
        PriorityQueue<int[]> ready = new PriorityQueue<>(new Comparator<int[]>(){
            public int compare(int[] a, int[] b){
                return a[1]-b[1];
            }
        });

        for (int[] job : jobs) {
            waiting.add(job);
        }
        
        int currTime = 0;
        int sum = 0;
        while (waiting.size() >0 || ready.size() > 0) {
            while (waiting.size() > 0 && waiting.peek()[0] <= currTime) {
                ready.add(waiting.poll());
            }
            if (ready.size() > 0) {
                int[] temp = ready.poll();
                currTime += temp[1];
                sum += currTime - temp[0];
            } else {
                currTime += 1;
            }
        }
        
        return sum / jobs.length;
    }
}
```
