# 주식가격

###### 문제 설명

초 단위로 기록된 주식가격이 담긴 배열 prices가 매개변수로 주어질 때, 가격이 떨어지지 않은 기간은 몇 초인지를 return 하도록 solution 함수를 완성하세요.

##### 제한사항

- prices의 각 가격은 1 이상 10,000 이하인 자연수입니다.
- prices의 길이는 2 이상 100,000 이하입니다.

##### 입출력 예

| prices          | return          |
| --------------- | --------------- |
| [1, 2, 3, 2, 3] | [4, 3, 1, 1, 0] |

##### 입출력 예 설명

- 1초 시점의 ₩1은 끝까지 가격이 떨어지지 않았습니다.
- 2초 시점의 ₩2은 끝까지 가격이 떨어지지 않았습니다.
- 3초 시점의 ₩3은 1초뒤에 가격이 떨어집니다. 따라서 1초간 가격이 떨어지지 않은 것으로 봅니다.
- 4초 시점의 ₩2은 1초간 가격이 떨어지지 않았습니다.
- 5초 시점의 ₩3은 0초간 가격이 떨어지지 않았습니다.

※ 공지 - 2019년 2월 28일 지문이 리뉴얼되었습니다.



---------------

  

## 권지민

> 현재 prices[i]와 비교하는 prices[j]의 간극을 재어 return하면 된다.  
> 단 바로 다음 주식에서 가격이 떨어지더라도 위의 예처럼 1초간은 가격이 떨어지지 않았으니 해당 부분만 유의하여 풀이하면 된다.

```java
class Solution {
    public int[] solution(int[] prices) {
        int len = prices.length;
        int[] answer = new int[len];
        int i, j;
        for (i = 0; i < len; i++) {
            for (j = i + 1; j < len; j++) {
                answer[i]++;
                if (prices[i] > prices[j])
                    break;
            }
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

> 오르고 있는 가격을 체크할 자료구조와 며칠 올랐는 지를 반환할 자료구조를 만든다.  
> 반복문을 돌면서 새 값을 rising에 넣고, 기존의 값이 내려갔다면 rising에서 뺀다.  
> rising에 있는 index로 반환할 자료구조의 값들(처음에는 0)을 1씩 더한다. 

```java
import java.util.*;

class Solution {
    public int[] solution(int[] prices) {
        int[] ret = new int[prices.length];
        
        ArrayList<Integer> rising = new ArrayList<>();
        ArrayList<Integer> removal = new ArrayList<>();

        for(int i=0; i<prices.length; i++){
            for(Integer n : rising) {
                if (prices[n.intValue()] <= prices[i]) {
                    ret[n]+=1;
                } else {
                    ret[n]+=1;
                    removal.add(n);
                }
            }
            for(Integer n : removal) {
                rising.remove(n);
            }
            removal.clear();
            rising.add(i);
        }
        return ret;
    }
}
```

