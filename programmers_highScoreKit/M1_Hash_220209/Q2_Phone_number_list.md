# 전화 번호 목록



###### 문제 설명

전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

- 구조대 : 119
- 박준영 : 97 674 223
- 지영석 : 11 9552 4421

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.

##### 제한 사항

- phone_book의 길이는 1 이상 1,000,000 이하입니다.
  - 각 전화번호의 길이는 1 이상 20 이하입니다.
  - 같은 전화번호가 중복해서 들어있지 않습니다.

##### 입출력 예제

| phone_book                        | return |
| --------------------------------- | ------ |
| ["119", "97674223", "1195524421"] | false  |
| ["123","456","789"]               | true   |
| ["12","123","1235","567","88"]    | false  |

##### 입출력 예 설명

입출력 예 #1
앞에서 설명한 예와 같습니다.

입출력 예 #2
한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

입출력 예 #3
첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다.

------

**알림**

2021년 3월 4일, 테스트 케이스가 변경되었습니다. 이로 인해 이전에 통과하던 코드가 더 이상 통과하지 않을 수 있습니다.

[출처](https://ncpc.idi.ntnu.no/ncpc2007/ncpc2007problems.pdf)



---------------





## 권지민

> 전화번호부 정렬
> phone_book을 순회하며 접두어를 뽑아낸다. (단, 접두어가 포함된 문자는 접두어보다 길이가 길어야 하므로 7번줄에 제어문 추가)
> substring로 prefix를 뽑아낸 후 접두어가 일치하는지 비교하여 맞으면 false 리턴, 반복문 다 돌때까지 없으면 true 리턴

```java
import java.util.Arrays;
class Solution {
    public boolean solution(String[] phone_book) {
        boolean answer = true;
        Arrays.sort(phone_book);
        for (int i = 0; i < phone_book.length - 1; i++) {
            if (phone_book[i + 1].length() > phone_book[i].length())
            {
                String prefix = phone_book[i + 1].substring(0,phone_book[i].length());
                if (phone_book[i].equals(prefix) == true)
                    return false;
            }   
        }
        return answer;
    }
}
```





## 김성용

> 대충 설명

```python
대충 코드
```





## 민성호

> 일단 전화부 문자열 배열을 정렬하고
> String 클래스의 startsWith 메서드를 활용

```java
import java.util.*;

class Solution {
    public boolean solution(String[] phone_book) {
        Arrays.sort(phone_book);
        for (int i=0; i<phone_book.length-1; i++) {
            if (phone_book[i+1].startsWith(phone_book[i])) {
                return false;
            }
        }
        return true;
    }
}
```

