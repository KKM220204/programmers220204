# 체육복



###### 문제 설명

점심시간에 도둑이 들어, 일부 학생이 체육복을 도난당했습니다. 다행히 여벌 체육복이 있는 학생이 이들에게 체육복을 빌려주려 합니다. 학생들의 번호는 체격 순으로 매겨져 있어, 바로 앞번호의 학생이나 바로 뒷번호의 학생에게만 체육복을 빌려줄 수 있습니다. 예를 들어, 4번 학생은 3번 학생이나 5번 학생에게만 체육복을 빌려줄 수 있습니다. 체육복이 없으면 수업을 들을 수 없기 때문에 체육복을 적절히 빌려 최대한 많은 학생이 체육수업을 들어야 합니다.

전체 학생의 수 n, 체육복을 도난당한 학생들의 번호가 담긴 배열 lost, 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열 reserve가 매개변수로 주어질 때, 체육수업을 들을 수 있는 학생의 최댓값을 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 전체 학생의 수는 2명 이상 30명 이하입니다.
- 체육복을 도난당한 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌의 체육복을 가져온 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌 체육복이 있는 학생만 다른 학생에게 체육복을 빌려줄 수 있습니다.
- 여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다. 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.

##### 입출력 예

| n    | lost   | reserve   | return |
| ---- | ------ | --------- | ------ |
| 5    | [2, 4] | [1, 3, 5] | 5      |
| 5    | [2, 4] | [3]       | 4      |
| 3    | [3]    | [1]       | 2      |

##### 입출력 예 설명

예제 #1
1번 학생이 2번 학생에게 체육복을 빌려주고, 3번 학생이나 5번 학생이 4번 학생에게 체육복을 빌려주면 학생 5명이 체육수업을 들을 수 있습니다.

예제 #2
3번 학생이 2번 학생이나 4번 학생에게 체육복을 빌려주면 학생 4명이 체육수업을 들을 수 있습니다.

[출처](http://hsin.hr/coci/archive/2009_2010/contest6_tasks.pdf)

※ 공지 - 2019년 2월 18일 지문이 리뉴얼되었습니다.
※ 공지 - 2019년 2월 27일, 28일 테스트케이스가 추가되었습니다.
※ 공지 - 2021년 7월 28일 테스트케이스가 추가되었습니다.
※ 공지 - 2021년 8월 30일 테스트케이스가 추가되었습니다.





---------------





## 권지민

> 학생수 n만큼 int student[n] = {1}; 선언 answer = n;   
>  그중 lost를 돌며 해당 index에 -1 reserver는 +1.  
> 값이 0이아닌 학생 전부 count.   
> 값이 0일때, 좌우로 값이 2가 없는 경우는 answer-- 빌려준 학생 옷 -1

```java
import java.util.Arrays;
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int answer = n;
        int []student = new int[n];
        
        Arrays.fill(student, 1);
        for (int i : lost)
            student[i - 1] -= 1;
        for (int i : reserve)
            student[i - 1] += 1;
        for (int i = 0; i < student.length; i++) {
            if (student[i] == 0) {
                if (i > 0 && student[i - 1] == 2) //왼쪽 검사
                    student[i - 1] -= 1;
                else if (i < n - 1 && student[i + 1] == 2)
                    student[i + 1] -= 1;
                else
                    answer--;
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

> 총원 - 잃어버린 사람 수.  

```java
import java.util.*;

class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int answer = n-lost.length;
        ArrayList<Integer> lostList = new ArrayList<>();
        for (int l : lost) {
            lostList.add(l);
        }
        
        ArrayList<Integer> reserveList = new ArrayList<>();
        for (int r : reserve) {
            reserveList.add(r);
        }
        Collections.sort(lostList);
        Collections.sort(reserveList);
        
        for (int l : lost) {
            if (reserveList.contains(l)) {
                reserveList.remove(reserveList.indexOf(l));
                lostList.remove(lostList.indexOf(l));
                answer++;
            }
        }
        
        for (int l : lostList) {
            if (reserveList.contains(l-1)) {
                reserveList.remove(reserveList.indexOf(l-1));
                answer++;
                continue;
            } else if (reserveList.contains(l+1)) {
                reserveList.remove(reserveList.indexOf(l+1));
                answer++;
            }
        }
        
        return answer;
    }
}
```
