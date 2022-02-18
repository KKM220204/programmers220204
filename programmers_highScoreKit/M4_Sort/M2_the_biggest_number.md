# 가장 큰 수



###### 문제 설명

0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.

예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.

0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

##### 제한 사항

- numbers의 길이는 1 이상 100,000 이하입니다.
- numbers의 원소는 0 이상 1,000 이하입니다.
- 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

##### 입출력 예

| numbers           | return    |
| ----------------- | --------- |
| [6, 10, 2]        | "6210"    |
| [3, 30, 34, 5, 9] | "9534330" |

------

※ 공지 - 2021년 10월 20일 테스트케이스가 추가되었습니다.





---------------





## 권지민

> 1. 들어온 숫자들을 맨 앞숫자 기준으로 4자리까지 채워준다. 2자리인 경우는 자기자신 그대로 붙여준다.   
> ex)1000 100 10 1 -> 1000 1001 1010 1111
> 2. Map<numbers인덱스, 1번에서 바꾼수>로 넣어두고 정렬한다.   
> 3. 큰 수 부터 key값의 인덱스를 가져와 붙여준다.  
> 3-1 . 만약 0인경우, charAt(0)으로 찾아 "0"반환.  
> (0000이 될 수 있기때문)

```java
class Solution {
    public String solution(int[] numbers) {
        String answer = "";
        HashMap<Integer, Integer> map = new HashMap<>();
        String [] nums = new String[numbers.length];
        String num;
        
        for (int i = 0; i < numbers.length; i++) {
            nums[i] = Integer.toString(numbers[i]);
        }
        for (int i = 0; i < nums.length; i++) {
            num = nums[i];
            if (num.length() == 2)
                num += num;
            else {
                for (int j = num.length(); j != 4; j++) {
                    num += nums[i].charAt(0);
                }
            }

            map.put(i, Integer.parseInt(num));
        }
        
        List<Map.Entry<Integer, Integer>> entryList = new LinkedList<>(map.entrySet());
        entryList.sort(new Comparator<Map.Entry<Integer, Integer>>() {
            @Override
            public int compare(Map.Entry<Integer, Integer> o1, Map.Entry<Integer, Integer> o2) {
           return o2.getValue() - o1.getValue();
            }
        });
        
        for (Map.Entry<Integer, Integer> entry : entryList) {
            answer += nums[entry.getKey()];
        }
        if (answer.charAt(0) == '0')
               return "0";
        return answer;
    }
}
```





## 김성용

> 코드

```python

```





## 민성호

> 주어진 정수 배열을 문자열 배열에 문자열로 변경해서 나눠 담는다.  
> 두 문자열을 합친 후 비교하는 기준으로 정렬을 한다.  
> 문자열 배열을 모두 합쳐서 문자열로 만든 후, 반환한다

```java
import java.util.*;

class Solution{
    public String solution(int[] numbers) {
        String[] strNum = new String[numbers.length];
        
        for(int i=0; i<strNum.length; i++) {
            strNum[i] = String.valueOf(numbers[i]);
        }
        Arrays.sort(strNum, new Comparator<String>(){
            public int compare(String sa, String sb){
                return (sb+sa).compareTo(sa+sb);
            }
        });
        
        if (strNum[0].equals("0")) return "0";
        
        String answer="";
        for(String str : strNum) {
            answer += str;
        }
        return answer;
    }
}
```
