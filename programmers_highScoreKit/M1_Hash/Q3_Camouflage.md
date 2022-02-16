# 위장



###### 문제 설명

스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.

예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

| 종류 | 이름                       |
| ---- | -------------------------- |
| 얼굴 | 동그란 안경, 검정 선글라스 |
| 상의 | 파란색 티셔츠              |
| 하의 | 청바지                     |
| 겉옷 | 긴 코트                    |

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
- 스파이는 하루에 최소 한 개의 의상은 입습니다.

##### 입출력 예

| clothes                                                      | return |
| ------------------------------------------------------------ | ------ |
| [["yellowhat", "headgear"], ["bluesunglasses", "eyewear"], ["green_turban", "headgear"]] | 5      |
| [["crowmask", "face"], ["bluesunglasses", "face"], ["smoky_makeup", "face"]] | 3      |

##### 입출력 예 설명

예제 #1
headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.

```
1. yellow_hat
2. blue_sunglasses
3. green_turban
4. yellow_hat + blue_sunglasses
5. green_turban + blue_sunglasses
```

예제 #2
face에 해당하는 의상이 crow_mask, blue_sunglasses, smoky_makeup이므로 아래와 같이 3개의 조합이 가능합니다.

```
1. crow_mask
2. blue_sunglasses
3. smoky_makeup
```

[출처](http://2013.bapc.eu/)



---------------





## 권지민

> 해시 문제  
> 의상의 종류 : key 의상의 갯수 : value  
> 같은 key가 이미 있는 경우, 해당 key의 value값을 카운트해준다.  
> a : 2  b : 1  c : 1 인 경우.  
> a1 a2 b c a1b a2b a1c a2c a1bc a2bc bc.  
> a는 a1, a2 0(안입음) b는 b 0 c는 c 0 이기 때문에 3 * 2 * 2 - 1(아무것도 안입는것)이다.

```java
import java.util.*;
class Solution {
    public int solution(String[][] clothes) {
        int answer = 1;
        HashMap<String, Integer> map = new HashMap<>();
        for (int i = 0; i < clothes.length; i++) {
            if (map.containsKey(clothes[i][1]))
            {
                int val = map.get(clothes[i][1]) + 1;
                map.put(clothes[i][1], val);
            }
            else
                map.put(clothes[i][1], 1);
        }
       for(String key : map.keySet()) {
            answer *= map.get(key) + 1;
           //System.out.println("{"+key+","+map.get(key)+"}");
       }
        
        return answer - 1;
    }
}
```





## 김성용

> 아무 의상을 착용하지 않는 경우 하나를 빼고 전부 계산해야함.  
> 의상의 종류별로 들어온 갯수를 dict로 센 다음 dict 순회하며 반복문 이용해 계산

```python
def solution(clothes):
    clothes_dict = dict()
    
    for clothe in clothes:
        if clothe[1] in clothes_dict:
            clothes_dict[clothe[1]] += 1
        else:
            clothes_dict[clothe[1]] = 1
    
    answer = 1
    for i in clothes_dict.values():
        answer *= (i + 1)
        
    return answer - 1
```





## 민성호

> [옷 종류 : [옷 이름] HashSet ] HashMap을 만든다  
> 인자로 받은 clothes 배열을 돌면서.  
> HashMap에 넣는다.  
> 종류 별로 옷이 몇 개 인지 세서 (각 c1, c2, c3, c4 ...개).  
> 계산한다 (c1+1) * (c2+1) * (c3+1) ... * (cn+1) -1   

```java
import java.util.*;

class Solution {
    public int solution(String[][] clothes) {
        HashMap<String, HashSet<String>> map = new HashMap<>();
        for(int i=0; i<clothes.length; i++) {
            String type = clothes[i][1];
            String name = clothes[i][0];
            HashSet<String> set=null;

            set=map.get(type);
            if(set==null) {
                set=new HashSet<>();
            }
            set.add(name);
            if(map.get(type)==null){
                map.put(type,set);
            } else {
                map.replace(type,set);
            }
        }
        int res=1;
        Iterator<String> ite=map.keySet().iterator();
        while (ite.hasNext()) {
            int size = map.get(ite.next()).size();
            res *= (size+1);
        }
        return res-1;
    }
}
```

HashMap의 Value로 굳이 HashSet 쓰지 말고Integer 쓸 걸