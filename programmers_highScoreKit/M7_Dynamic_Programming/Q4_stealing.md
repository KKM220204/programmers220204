# 도둑질



###### 문제 설명

도둑이 어느 마을을 털 계획을 하고 있습니다. 이 마을의 모든 집들은 아래 그림과 같이 동그랗게 배치되어 있습니다.

![image.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/e7dd4f51c3/a228c73d-1cbe-4d59-bb5d-833fd18d3382.png)

각 집들은 서로 인접한 집들과 방범장치가 연결되어 있기 때문에 인접한 두 집을 털면 경보가 울립니다.

각 집에 있는 돈이 담긴 배열 money가 주어질 때, 도둑이 훔칠 수 있는 돈의 최댓값을 return 하도록 solution 함수를 작성하세요.

##### 제한사항

- 이 마을에 있는 집은 3개 이상 1,000,000개 이하입니다.
- money 배열의 각 원소는 0 이상 1,000 이하인 정수입니다.

##### 입출력 예

| money        | return |
| ------------ | ------ |
| [1, 2, 3, 1] | 4      |





---------------





## 권지민

> 1. 첫 번째 집과 두 번째 집을 터는 경우로 두 가지로 나뉜다.
> 2. first, second 배열을 만들어 각각 집마다 index별로 money를 추가해준다.
> 3. 맨 앞부터 집 3개의 값을 초기화 시켜준다.   
> first의 경우 2번째 집의 돈을 -1로 하여 무조건 첫번째 집을 고르도록 하고, second의 경우 첫 번째 집의 돈을 -1로하여 무조건 두 번째 집을 고르게 한다.
> 4. 4번째 집부터 시작하여 (집이 3개인 경우는 3번째 집에서 정해짐).  
> 내 전전과 전전전의 값을 비교하여 큰 값을 합산한다.   
> 5.반복문이 끝나고 first, second중 큰 값을 return

```java
class Solution {
    public int solution(int[] money) {
        int[] dp_first = new int[money.length];
        int[] dp_second = new int[money.length];
        
        for(int i = 0; i < money.length; i++) {
            dp_first[i] = money[i];
            dp_second[i] = money[i];
        }
        dp_first[1] = -1;
        dp_second[0] = -1;
        dp_first[2] += dp_first[0];
        for (int i = 3; i < money.length; i++) {
            dp_first[i] += Math.max(dp_first[i - 2], dp_first[i - 3]);
            dp_second[i] += Math.max(dp_second[i - 2], dp_second[i - 3]);
        }
        int first_max = Math.max(dp_first[money.length - 2], dp_first[money.length - 3]);
        int second_max = Math.max(dp_second[money.length - 1], dp_second[money.length - 2]);
        return Math.max(first_max, second_max);
    }
}
```





## 김성용

> 코드

```python

```





## 민성호

> 첫째 집을 터는 경우, 아닌 경우 두 가지로 나누어(even, odd).  
> 쌓아나가면서 계산한다

```java
import java.util.*;

class Solution {
    public int solution(int[] money) {
        int len = money.length;
        int[] odd = new int[len];
        int[] even = new int[len];
        
        
        
        odd[0] = money[0];
        odd[1] = money[0];
        for (int i=2; i<len-1; i++) {
            odd[i] = Math.max(odd[i-2] + money[i], odd[i-1]);
        }
        
        even[0] = 0;
        even[1] = money[1];
        for (int i=2; i<len; i++) {
            even[i] = Math.max(even[i-2]+money[i], even[i-1]);
        }
        
        return Math.max(odd[len-2] , even[len-1]);
    }
}
```
