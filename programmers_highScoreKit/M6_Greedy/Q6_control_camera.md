# 단속 카메라



###### 문제 설명

고속도로를 이동하는 모든 차량이 고속도로를 이용하면서 단속용 카메라를 한 번은 만나도록 카메라를 설치하려고 합니다.

고속도로를 이동하는 차량의 경로 routes가 매개변수로 주어질 때, 모든 차량이 한 번은 단속용 카메라를 만나도록 하려면 최소 몇 대의 카메라를 설치해야 하는지를 return 하도록 solution 함수를 완성하세요.

**제한사항**

- 차량의 대수는 1대 이상 10,000대 이하입니다.
- routes에는 차량의 이동 경로가 포함되어 있으며 routes[i][0]에는 i번째 차량이 고속도로에 진입한 지점, routes[i][1]에는 i번째 차량이 고속도로에서 나간 지점이 적혀 있습니다.
- 차량의 진입/진출 지점에 카메라가 설치되어 있어도 카메라를 만난것으로 간주합니다.
- 차량의 진입 지점, 진출 지점은 -30,000 이상 30,000 이하입니다.

**입출력 예**

| routes                                    | return |
| ----------------------------------------- | ------ |
| [[-20,-15], [-14,-5], [-18,-13], [-5,-3]] | 2      |

**입출력 예 설명**

-5 지점에 카메라를 설치하면 두 번째, 네 번째 차량이 카메라를 만납니다.

-15 지점에 카메라를 설치하면 첫 번째, 세 번째 차량이 카메라를 만납니다.





---------------





## 권지민

> 단속카메라.  
> routes[i][0]번 기준으로 정렬한다.   
> firstIndex : 겹치는 차량 첫번째 index.  
> isOverlap으로 나 자신부터 firstIndex차량까지 순회하며 시간대가 겹치면 true.  
> 1,2,3번 차량이 겹치고 4번은 안겹치면 그 다음에는 firstIndex : 4.  
> 5번 차량부터 firstIndex(4번)차량과 겹치면 6,7...로 겹치는 차량이 아닐때까지 반복

```java
import java.util.Arrays;
import java.util.Comparator;
class Solution {
    public boolean isOverlap(int[][] routes, int index, int firstIndex) {
        for(int i = index - 1; i >= firstIndex; i--) {
            if (!(routes[index][0] >= routes[i][0] &&
               routes[index][0] <= routes[i][1]))
                return false;
        }
        return true;
    }
    public int solution(int[][] routes) {
        int answer = 0;
        
        Arrays.sort(routes, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[]o2) {
                return o1[0] - o2[0];
            }
        });
        if (routes.length == 1) return 1;
        int index = 0;
        while(index < routes.length) {
            int firstIndex = index;
            answer++;
            index++;
            int overlap = 0; //해당 시점에 겹치는 자동차 개수
            while(index < routes.length) {
                int time = routes[index][0];
                if(isOverlap(routes, index, firstIndex) == true) {
                    overlap++;
                    index++;
                }
                else 
                    break;
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

> 나오는 게이트 기준으로 정렬 (간트차트 처럼).  
> 반복문을 돌린다.   

```java
import java.util.Arrays;

class Solution {
    public int solution(int[][] routes) {
		Arrays.sort(routes, (a,b)-> Integer.compare(a[1], b[1]));
		int cnt = 0;
		
		int min = Integer.MIN_VALUE;
		for(int[] route : routes) {
			if(min <  route[0] ) {
				min = route[1];
				++cnt;
			}
		}
        return cnt;
    }
}
```
