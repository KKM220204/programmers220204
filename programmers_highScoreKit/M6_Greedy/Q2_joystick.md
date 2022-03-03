# 조이스틱



###### 문제 설명

조이스틱으로 알파벳 이름을 완성하세요. 맨 처음엔 A로만 이루어져 있습니다.
ex) 완성해야 하는 이름이 세 글자면 AAA, 네 글자면 AAAA

조이스틱을 각 방향으로 움직이면 아래와 같습니다.

```
▲ - 다음 알파벳
▼ - 이전 알파벳 (A에서 아래쪽으로 이동하면 Z로)
◀ - 커서를 왼쪽으로 이동 (첫 번째 위치에서 왼쪽으로 이동하면 마지막 문자에 커서)
▶ - 커서를 오른쪽으로 이동 (마지막 위치에서 오른쪽으로 이동하면 첫 번째 문자에 커서)
```

예를 들어 아래의 방법으로 "JAZ"를 만들 수 있습니다.

```
- 첫 번째 위치에서 조이스틱을 위로 9번 조작하여 J를 완성합니다.
- 조이스틱을 왼쪽으로 1번 조작하여 커서를 마지막 문자 위치로 이동시킵니다.
- 마지막 위치에서 조이스틱을 아래로 1번 조작하여 Z를 완성합니다.
따라서 11번 이동시켜 "JAZ"를 만들 수 있고, 이때가 최소 이동입니다.
```

만들고자 하는 이름 name이 매개변수로 주어질 때, 이름에 대해 조이스틱 조작 횟수의 최솟값을 return 하도록 solution 함수를 만드세요.

##### 제한 사항

- name은 알파벳 대문자로만 이루어져 있습니다.
- name의 길이는 1 이상 20 이하입니다.

##### 입출력 예

| name     | return |
| -------- | ------ |
| "JEROEN" | 56     |
| "JAN"    | 23     |

[출처](https://commissies.ch.tudelft.nl/chipcie/archief/2010/nwerc/nwerc2010.pdf)

※ 공지 - 2019년 2월 28일 테스트케이스가 추가되었습니다.
※ 공지 - 2022년 1월 14일 지문 수정 및 테스트케이스가 추가되었습니다. 이로 인해 이전에 통과하던 코드가 더 이상 통과하지 않을 수 있습니다.





---------------





## 권지민

> 알파벳은 총 26개, 두 알파벳 사이의 간격은  알파벳2 - 알파벳1 or 26 - (알파벳2 - 알파벳1).  
> 위로 조작 : 해당하는 알파벳 - 'A'.  
> 아래로 조작 : 26 - 위로 조작한 횟수.  
>
> 1. A가 끝나는 지점을 endA변수에. 구함. 그 지점까지 이동하는데 걸리는 값 왼, 오 비교
>
> 2. 더 짧은 방향을 move에 넣어줌. 가장 작은 move 마지막에 반환

```java
import java.util.Arrays;
class Solution {
    public int findAlpha(char c) {
        int up = c - 'A';
        int down = 26 - up;
        return up > down ? down : up;
    }
    public int solution(String name) {
        int answer = 0;
        int move = name.length() - 1; // 오른쪽으로 쭉 간 횟수
        
        for(int i = 0; i < name.length(); i++) {
            answer += findAlpha(name.charAt(i)); //상,하 알파벳 맞추기
            if (i < name.length() - 1 && name.charAt(i + 1) == 'A') {
                int endA = i + 1;
                while(endA < name.length() && name.charAt(endA) == 'A')
                    endA++;
                if (move > i * 2 + (name.length() - endA)) // 오른쪽으로 갔다 다시 왼쪽으로 꺾기
                    move = i * 2 + (name.length() - endA);
                if (move > i + (name.length() - endA) * 2) // 왼쪽으로 갔다 다시 오른쪽으로 꺾기
                    move = i + (name.length() - endA) * 2;
            }
        }
        return answer + move;
    }
}
```





## 김성용

> 코드

```python

```





## 민성호

> 좌우, 상하 이동을 따로 계산

```java
class Solution {
    public int solution(String name) {       
        int upDown=0, len=name.length();
        for(int charAt : name.toCharArray()){
            upDown += (charAt> 78)? 91-charAt: charAt-65;
        }
        int leftRight = len-1;
        for(int i=0;i<len;i++){
            int next=i+1;
            while(next<len && name.charAt(next)=='A') next++;
            leftRight = Math.min(leftRight,i+len-next +Math.min(i,len-next));
        }
        return upDown+leftRight;
    }
}
```
