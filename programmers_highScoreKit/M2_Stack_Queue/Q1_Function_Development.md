# 기능 개발

###### 문제 설명

프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

##### 제한 사항

- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

##### 입출력 예

| progresses               | speeds             | return    |
| ------------------------ | ------------------ | --------- |
| [93, 30, 55]             | [1, 30, 5]         | [2, 1]    |
| [95, 90, 99, 99, 80, 99] | [1, 1, 1, 1, 1, 1] | [1, 3, 2] |

##### 입출력 예 설명

입출력 예 #1
첫 번째 기능은 93% 완료되어 있고 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포가 가능합니다.
두 번째 기능은 30%가 완료되어 있고 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포가 가능합니다. 하지만 이전 첫 번째 기능이 아직 완성된 상태가 아니기 때문에 첫 번째 기능이 배포되는 7일째 배포됩니다.
세 번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능합니다.

따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포됩니다.

입출력 예 #2
모든 기능이 하루에 1%씩 작업이 가능하므로, 작업이 끝나기까지 남은 일수는 각각 5일, 10일, 1일, 1일, 20일, 1일입니다. 어떤 기능이 먼저 완성되었더라도 앞에 있는 모든 기능이 완성되지 않으면 배포가 불가능합니다.

따라서 5일째에 1개의 기능, 10일째에 3개의 기능, 20일째에 2개의 기능이 배포됩니다.

※ 공지 - 2020년 7월 14일 테스트케이스가 추가되었습니다.



---------------

  

## 권지민

> 주식가격.   
> 매일 각 progresses에 담긴 작업에 speeds만큼 작업량을 올려준다.  
> que의 맨 앞에 있는 작업량이 100이상일 때, 해당 작업을 포함하여 연달아 작업량을 완수한게 있다면 poll로 뽑아낸다.  
> ex) 예제를 보면 7일 째 작업량은 [100, 240, 90] 이므로 1번 째, 두번 째 배포한 후, 2일 후 3번 째 작업을 배포한다.

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        int complete;
        int days = 1;
        ArrayList<Integer> array = new ArrayList<>();
        Queue<Integer> que = new LinkedList<>();
        Queue<Integer> spd = new LinkedList<>();
        
        for (int i = 0; i < progresses.length; i++)
            que.add(progresses[i]);
        for (int i = 0; i < speeds.length; i++)
            spd.add(speeds[i]);
        
        while (!que.isEmpty()) {
            complete = 0;
            if (que.peek() + spd.peek() * days  >= 100) {
                while(!que.isEmpty() && que.peek() + spd.peek() * days >= 100) {
                    complete++;
                    que.poll();
                    spd.poll();
                }
                array.add(complete);
            }
            days++;
        }
        int[] answer = new int[array.size()];
        for (int i = 0; i < array.size(); i++) {
            answer[i] = array.get(i);
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

> 진척도와 스피드를 저장하는 자료구조를 만든다.  
> 매일 진척도에 스피드를 더한다.  
> 만약 가장 앞 자료의 진척도가 100이상이면,  
> 가장 앞에서부터 연속적으로 100 이상인 자료들을 모두 제거한다.  
> 제거한 날짜들을 배열에 저장했다가, 반환한다

```java
import java.util.*;

class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        ArrayList<Integer> list = new ArrayList<>();
        ArrayList<Integer> speed = new ArrayList<>();
        ArrayList<Integer> ret = new ArrayList<>();
        
        for (int n : progresses) {
            list.add(n);
        }
        for (int n : speeds) {
            speed.add(n);
        }
        boolean check=false;
        while (list.size()>0) {
            int cnt = 0;
            while (list.size()>0 && list.get(0) >= 100){
                list.remove(0);
                speed.remove(0);
                cnt++;
                check=true;
            }
            if(check){
                ret.add(cnt);
                check=false;
            }
            
            for(int i=0; i<list.size(); i++) {
                list.set(i,list.get(i)+speed.get(i));
            }
        }
        int[] retArr = new int[ret.size()];
        int i=0;
        for (Integer n : ret) {
            retArr[i]=n;
            i++;
        }
        return retArr;
    }
}
```

