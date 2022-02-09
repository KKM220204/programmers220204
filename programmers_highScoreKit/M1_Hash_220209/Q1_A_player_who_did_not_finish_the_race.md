# 완주하지 못한 선수



###### 문제 설명

수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

##### 입출력 예

| participant                                       | completion                               | return   |
| ------------------------------------------------- | ---------------------------------------- | -------- |
| ["leo", "kiki", "eden"]                           | ["eden", "kiki"]                         | "leo"    |
| ["marina", "josipa", "nikola", "vinko", "filipa"] | ["josipa", "filipa", "marina", "nikola"] | "vinko"  |
| ["mislav", "stanko", "mislav", "ana"]             | ["stanko", "ana", "mislav"]              | "mislav" |

##### 입출력 예 설명

예제 #1
"leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #2
"vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #3
"mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

[출처](http://hsin.hr/coci/archive/2014_2015/contest2_tasks.pdf)



---------------





## 권지민

> participant와 completion 둘 다 정렬
> 정렬된 두 배열을 비교하면서 틀린 경우 해당 참가자를 반환.
> completion을 다 돌고도 없다면 맨 마지막 선수가 완주하지 못한 선수에 해당하므로 마지막 participant 반환

```java
import java.util.Arrays;
// a a b b c d 
// a a b c c d
class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";
        Arrays.sort(participant);
        Arrays.sort(completion);
        int i;
        for (i = 0 ; i < completion.length; i++) {
            if(!completion[i].equals(participant[i])) {
                return participant[i];
            }
        }
        return participant[i];
    }
}
```





## 김성용

> 대충 설명

```python
대충 코드
```





## 민성호

> [선수 이름 : 사람 수] 구조의 HashMap을 만들어서
> praticipant 배열을 순회하면서 해시맵에 추가 (있으면 value+=1)
> completion 배열을 순회하면서 해시맵 값 변경 (value-=1)
> 마지막에 map을 돌면서 value가 0이 아닌 사람 찾아서 반환

```java
import java.util.*;

class Solution {
    public String solution(String[] participant, String[] completion) {
        HashMap<String,Integer> hashMap = new HashMap<>();
        
        for(String person : participant) {
            if (hashMap.get(person)==null){
                hashMap.put(person,1);
            } else {
                hashMap.put(person,hashMap.get(person)+1);
            }
        }
        for (String personCompleted : completion) {
            hashMap.replace(personCompleted,hashMap.get(personCompleted)-1);
        }
        Set<String> key=hashMap.keySet();
        Iterator<String> ite=key.iterator();
        while(ite.hasNext()) {
            String res=ite.next();
            if(hashMap.get(res)!=0) {
                return res;
            }
        }
        return "";
    }
}
```

해시맵의 contatinsKey메서드를 사용하면 더 편리하다