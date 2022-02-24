# 모의고사



###### 문제 설명

수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

##### 제한 조건

- 시험은 최대 10,000 문제로 구성되어있습니다.
- 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

##### 입출력 예

| answers     | return  |
| ----------- | ------- |
| [1,2,3,4,5] | [1]     |
| [1,3,2,4,2] | [1,2,3] |

##### 입출력 예 설명

입출력 예 #1

- 수포자 1은 모든 문제를 맞혔습니다.
- 수포자 2는 모든 문제를 틀렸습니다.
- 수포자 3은 모든 문제를 틀렸습니다.

따라서 가장 문제를 많이 맞힌 사람은 수포자 1입니다.

입출력 예 #2

- 모든 사람이 2문제씩을 맞췄습니다.



---------------





## 권지민

> 각 수포자가 문제를 찍는 규칙을 배열로 만들어둔다.  
> 1번 수포자는 [1, 2, 3, 4, 5].  
> 2번 수포자는 [2, 1, 2, 3, 2, 4, 2, 5].  
> 3번 수포자는 [3, 3, 1, 1, 2, 2, 4, 4, 5, 5].  
> 정답과 해당 규칙을 비교해서 각각 정답 수를 count.  
> 단, 높은 점수가 동률일 경우 오름차순으로 정렬해서 보내줘야 하므로 반복문을 오름차순 기준으로 두어 수포자의 점수와 높은 점수를 비교하여 ArrayList에 수포자를 넣는다.

```java
import java.util.Arrays;
import java.util.ArrayList;

class Solution {
    public int[] solution(int[] answers) {
        int[] answer = {0, 0, 0};
        int[] one = {1, 2, 3, 4, 5};
        int[] two = {2, 1, 2, 3, 2, 4, 2, 5};
        int[] three = {3, 3, 1, 1, 2, 2, 4, 4, 5, 5};
        ArrayList<Integer> ans = new ArrayList<>();
        
        for (int i = 0; i < answers.length; i++) {
            if (answers[i] == one[i % 5])
                answer[0]++;
            if (answers[i] == two[i % 8])
                answer[1]++;
            if (answers[i] == three[i % 10])
                answer[2]++;
        }
        int[] max = Arrays.copyOfRange(answer, 0, 3);
        Arrays.sort(max);
        for (int i = 0; i < 3; i++) {
            if (answer[i] == max[2])
                ans.add(i + 1);
        }
        int [] ret = new int[ans.size()];
        for (int i = 0; i < ans.size(); i++)
            ret[i] = ans.get(i);
        return ret;
    }
}
```





## 김성용

> 코드

```python

```





## 민성호

> 수포자들의 맞춘 문제 수가 담겨 있는 배열을 만들고.  
> 답지를 순회하면서 인덱스 나머지 연산을 통해 해당 문제가 맞았는지 체크한다.  
> 각 조건문을 돌면서 수포자들이 정답을 맞춘 경우 배열의 값을 증가 시킨다.   
> 리턴 형식에 맞춰 반환한다.

```java
import java.util.ArrayList;

class Solution {
    public int[] solution(int[] answers) {
        int[] supo1 = new int[]{1,2,3,4,5};
        int[] supo2 = new int[]{2,1,2,3,2,4,2,5};
        int[] supo3 = new int[]{3,3,1,1,2,2,4,4,5,5};
        
        int[] check = {0,0,0};
        for (int i=0; i<answers.length; i++) {
            if (answers[i] == supo1[i%supo1.length]) {
                check[0]++;
            }
            if (answers[i] == supo2[i%supo2.length]) {
                check[1]++;
            }
            if (answers[i] == supo3[i%supo3.length]) {
                check[2]++;
            }
        }
        
        ArrayList<Integer> list = new ArrayList<>();
        if (Math.max(check[0],Math.max(check[1],check[2]))==check[0]) {
            list.add(1);
        }
        if (Math.max(check[1],Math.max(check[0],check[2]))==check[1]) {
            list.add(2);
        }
        if (Math.max(check[2],Math.max(check[0],check[1]))==check[2]) {
            list.add(3);
        }
        
        int[] ret = new int[list.size()];
        for (int i=0; i<ret.length; i++) {
            ret[i] = list.get(i);
        }
        return ret;
    }
}
```
