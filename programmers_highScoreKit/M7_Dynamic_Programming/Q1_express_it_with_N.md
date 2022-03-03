# N으로 표현



###### 문제 설명

아래와 같이 5와 사칙연산만으로 12를 표현할 수 있습니다.

12 = 5 + 5 + (5 / 5) + (5 / 5)
12 = 55 / 5 + 5 / 5
12 = (55 + 5) / 5

5를 사용한 횟수는 각각 6,5,4 입니다. 그리고 이중 가장 작은 경우는 4입니다.
이처럼 숫자 N과 number가 주어질 때, N과 사칙연산만 사용해서 표현 할 수 있는 방법 중 N 사용횟수의 최솟값을 return 하도록 solution 함수를 작성하세요.

##### 제한사항

- N은 1 이상 9 이하입니다.
- number는 1 이상 32,000 이하입니다.
- 수식에는 괄호와 사칙연산만 가능하며 나누기 연산에서 나머지는 무시합니다.
- 최솟값이 8보다 크면 -1을 return 합니다.

##### 입출력 예

| N    | number | return |
| ---- | ------ | ------ |
| 5    | 12     | 4      |
| 2    | 11     | 3      |

##### 입출력 예 설명

예제 #1
문제에 나온 예와 같습니다.

예제 #2
`11 = 22 / 2`와 같이 2를 3번만 사용하여 표현할 수 있습니다.

[출처](https://www.oi.edu.pl/old/php/show.php?ac=e181413&module=show&file=zadania/oi6/monocyfr)

※ 공지 - 2020년 9월 3일 테스트케이스가 추가되었습니다.





---------------





## 권지민

> 1개 : 5.  
> 2개 : 5 + 5, 5 / 5, 5 - 5, 5 * 5, 55.  
> 3개 : 1개 + 2개 , 2개 + 1개 (괄호를 생각하면 두 경우가 다르다.) + 555.  
> n개 : 더해서 a + b = n이 되는 (a, b)순서쌍. 합칠 때 사칙연산 별로 모두 넣는다. + 5연속된 n개만큼 1 2.  
> 1~8개수 별로 만들 수 있는 모든 경우를 다 넣고, 1개 있는 통부터 number과 비교

```java
import java.util.HashSet;
import java.util.Set;
import java.util.ArrayList;
import java.util.List;
class Solution {
    //순서쌍 a에 b 합치기
    public void unionSet(Set<Integer> union, Set<Integer> a, Set<Integer> b) {
        for (int n1 : a) {
            for (int n2 : b) {
                union.add(n1 + n2);
                union.add(n1 - n2);
                union.add(n1 * n2);
                if (n2 != 0)
                    union.add(n1 / n2);
            }
        }
    }
    public int solution(int N, int number) {
        List<Set<Integer>> setList = new ArrayList<>();
        
        for (int i = 0; i < 9; i++)
            setList.add(new HashSet<>()); // 개수 별 해쉬셋
        setList.get(1).add(N); //1개로 만들 수 있는 건 나 자신뿐
        if (number == N)
            return 1;
        for (int i = 2; i < 9; i++) {
            for (int j = 1; j <= i / 2; j++) {
                unionSet(setList.get(i), setList.get(i - j), setList.get(j));
                unionSet(setList.get(i), setList.get(j), setList.get(i - j));
            }
            String n = Integer.toString(N);
            setList.get(i).add(Integer.parseInt(n.repeat(i))); //연속된 숫자 넣기
            for (int num : setList.get(i))
                if (num == number)
                    return i;
        }
        return -1;
    }
}
```





## 김성용

> 코드

```python

```





## 민성호

> 사칙 연산, 숫자를 연속해서 쓰는 경우를 dfs로 돌린다

```java
class Solution {
    int answer=-1;
    public int solution(int N, int number) {
        dfs(0, N, number,0);
        return answer;
    }
    
    public void dfs(int init, int N, int number, int cnt) {
        if (cnt > 8) return ;
        if (init==number) {
            if (answer==-1) {
                answer=cnt;
            } else {
                answer = Math.min(answer,cnt);
            }
        }
        int x = N;
        for (int i=1; i<=8-cnt; i++) {
            dfs(init+x, N, number, cnt+i);
            dfs(init-x, N, number, cnt+i);
            dfs(init*x, N, number, cnt+i);
            dfs(init/x, N, number, cnt+i);
            x = (x*10) + N;
        }
    }
}
```
