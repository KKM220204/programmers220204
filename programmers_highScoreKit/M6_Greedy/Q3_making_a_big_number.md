# 큰 수 만들기



###### 문제 설명

어떤 숫자에서 k개의 수를 제거했을 때 얻을 수 있는 가장 큰 숫자를 구하려 합니다.

예를 들어, 숫자 1924에서 수 두 개를 제거하면 [19, 12, 14, 92, 94, 24] 를 만들 수 있습니다. 이 중 가장 큰 숫자는 94 입니다.

문자열 형식으로 숫자 number와 제거할 수의 개수 k가 solution 함수의 매개변수로 주어집니다. number에서 k 개의 수를 제거했을 때 만들 수 있는 수 중 가장 큰 숫자를 문자열 형태로 return 하도록 solution 함수를 완성하세요.

##### 제한 조건

- number는 1자리 이상, 1,000,000자리 이하인 숫자입니다.
- k는 1 이상 `number의 자릿수` 미만인 자연수입니다.

##### 입출력 예

| number       | k    | return   |
| ------------ | ---- | -------- |
| "1924"       | 2    | "94"     |
| "1231234"    | 3    | "3234"   |
| "4177252841" | 4    | "775841" |

[출처](http://hsin.hr/coci/archive/2011_2012/contest4_tasks.pdf)





---------------





## 권지민

> - 만들 수의 길이 len을 구한다.
> - 먼저 가장 큰 수가 되려면 len길이가 보장되어야 한다.
> 즉, 111188일때, 가장 큰 수 8을 먼저 뽑아도 len이 4면 88로 2자리기 때문에 성립하지 못한다.
> - 따라서 len = n 일때, n-1개의 숫자는 미리 예비로 빼두고 남은 숫자에서 max를 구한다.
> - "4177252841” k = 4일때 len = 6
> 맨 첫숫자 후보 중 가장 큰 걸 뽑아야한다.  
> 5개는 뒤로 빼두고 41772중 3번째 7이 max로 선택. “77”.  
> 7의 index = 2.   
> 이제 index = 3부터 ‘725’중 max 찾는다. (len 보장을 위해 2841은 빼둔다.).  
> 7선택, index = 3.  
> index = 4부터 ‘252’중 max 찾는다. (841)은 빼둔다.   
> 5선택, index = 5.  
> index = 6부터 ‘28’중 max 찾는다. (41)은 빼둔다.   
> 8선택, index = 7.  
> index = 8부터 ‘4’중 max 찾는다. (1)은 빼둔다.   
> 4선택 후 남은 1 선택.   
> 따라서 ‘7 7 5 8 4 1’이 된다.

```java
class Solution {
    public String solution(String number, int k) {
        StringBuilder answer = new StringBuilder("");
        int len = number.length() - k;
        int start = 0;
        
        while(start < number.length() && answer.length() != len) {
            int leftNum = k + answer.length() + 1;
            int max = 0;
            for (int j = start; j < leftNum; j++) {
                if (max < number.charAt(j) - '0') {
                    max = number.charAt(j) - '0';
                    start = j + 1;
                }
            }
            answer.append(Integer.toString(max));
        }
        return answer.toString();
    }
}
```





## 김성용

> 코드

```python

```





## 민성호

> 최소 길이를 계산해서, 그 바로 앞에서 부터.  
> 돌아가면서 교체

```java
import java.util.*;

class Solution {
    int num = 0;
    
    public String solution(String number, int k) {
        if (number.charAt(0)=='0') return "0";
        
        StringBuilder answer=new StringBuilder();
        int next=0;
        
        for (int i=0; i<number.length()-k; i++) {
            char max = '0';
            for (int j=next; j<=k+i; j++) {
                if (number.charAt(j)>max) {
                    max = number.charAt(j);
                    next = j+1;
                }
            }
            answer.append(max);
        }
        return answer.toString();
    }
}
```
