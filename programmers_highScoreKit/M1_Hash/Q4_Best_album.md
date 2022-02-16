# 베스트앨범



###### 문제 설명

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

##### 제한사항

- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

##### 입출력 예

| genres                                          | plays                      | return       |
| ----------------------------------------------- | -------------------------- | ------------ |
| ["classic", "pop", "classic", "classic", "pop"] | [500, 600, 150, 800, 2500] | [4, 1, 3, 0] |

##### 입출력 예 설명

classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.

- 고유 번호 3: 800회 재생
- 고유 번호 0: 500회 재생
- 고유 번호 2: 150회 재생

pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.

- 고유 번호 4: 2,500회 재생
- 고유 번호 1: 600회 재생

따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.

※ 공지 - 2019년 2월 28일 테스트케이스가 추가되었습니다.



---------------





## 권지민

> 해시맵을 이용해 <genres, plays>로 plays를 중첩하여 1번 기준을 먼저 찾는다.  
> 찾은 1번 기준에 맞춘 순서로 sortedGenres String 배열에 넣는다.  
> mostSong<Integer, Integer> 해시맵 안에 장르에 해당하는 노래 index, 재생 수를 넣고 또 정렬한다.  
> mostSong 상위 2개가 2번에 해당된다.  
> 3번은 mostSong 해시맵 정렬을 하며 기본적으로 오름차순이기 때문에 고려하지 않아도 된다.

```java
import java.util.HashMap;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.LinkedList;
import java.util.Comparator;
// 해시맵을 이용해 <genres, plays>로 plays를 중첩하여 1번 기준을 먼저 찾는다.
// 해시맵 정렬을 이용하여 장르 순서대로 String 배열에 넣는다.
// 모든 장르를 돌며 해시맵 <인덱스, plays수>를 넣고, 똑같이 value 기준으로 정렬해서 맨 앞 두개 index를 넣는다.
// 다만 해시맵 size == 1인경우 key값 하나만 넣는다.
class Solution {
    public int[] solution(String[] genres, int[] plays) {
        int[] answer = {};
        ArrayList<Integer> ans = new ArrayList<>();
        HashMap<String, Integer> map = new HashMap<>();
        
        for (int i = 0; i < genres.length; i++) {
            if (map.containsKey(genres[i])) {
                int play = map.get(genres[i]);
                map.put(genres[i], play + plays[i]);
            }
            else
                map.put(genres[i], plays[i]);
        }
        String []sortedGenres = new String[map.size()];
        List<Map.Entry<String, Integer>> entryList = new LinkedList<>(map.entrySet());
        entryList.sort(new Comparator<Map.Entry<String, Integer>>() {
        @Override
        public int compare(Map.Entry<String, Integer> o1, Map.Entry<String, Integer> o2) {
            return o2.getValue() - o1.getValue();
            }
        });
        int i = 0;
        for(Map.Entry<String, Integer> entry : entryList){
            System.out.println("key : " + entry.getKey() + ", value : " + entry.getValue());
            sortedGenres[i++] = entry.getKey();
        }
        // 모든 장르를 돌며 해시맵 <인덱스, plays수>를 넣는다.
        HashMap<Integer, Integer> mostSong = new HashMap<>();
        for (int j = 0; j < sortedGenres.length; j++) {
            for (int z = 0; z < genres.length; z++) {
                if (genres[z].equals(sortedGenres[j])) {
                    mostSong.put(z, plays[z]);
                }
            }
            // play 순서별로 정렬
            List<Map.Entry<Integer, Integer>> entryList2 = new LinkedList<>(mostSong.entrySet());
            entryList2.sort(new Comparator<Map.Entry<Integer, Integer>>() {
            @Override
            public int compare(Map.Entry<Integer, Integer> o1, Map.Entry<Integer, Integer> o2) {
                return o2.getValue() - o1.getValue();
                }
            });
            i = 0;
            for(Map.Entry<Integer, Integer> entry : entryList2){
                if (i == 2)
                    break;
                ans.add(entry.getKey());
                i++;
            }   
            mostSong.clear();
        }
        answer = new int[ans.size()];
        for (i = 0; i < ans.size(); i++) {
            answer[i] = ans.get(i);
        }
        return answer;
    }
}
```





## 김성용

> 문제 조건에 맞게 반환값을 구하기 위해 play_info에 dict형태로 저장.  
> play_info에 저장된 데이터는 다음과 같다.  
> { key = 장르이름,    
>   value = [ 해당 장르의 play시간들의 합, { key = 노래의 고유번호 i,  
>                                                                    value = 플레이시간(plays[i]) } ]. 
>
> 반복문으로 genres, plays에 들어있는 데이터를 play_info에 모두 저장후.  
> 문제 조건에 맞게 play_info를 '해당 장르의 play시간들의 합'의 내림차순으로 정렬.  
> 각각의 장르에서 노래의 { key = 노래의 고유번호 i, value = 플레이시간(plays[i]) }을 플레이시간의 내림차순으로 정렬.  
> album에 재생시간 합산이 높은 장르부터 가장 많이 재생된 노래 두곡을 append 해준후 return.  
>
> 이때 장르별로 제일 플레이가 많이 된 노래 2개씩만 앨범에 수록하면 되므로,  
> plays의 데이터를 play_info에 저장할때 제일 플레이가 많이 된 노래 2개를 찾아 같이 저장해놓도록 로직을 고친다면, 마지막에 genre_info별로 play_time을 sort할 필요가 없어지므로 속도가 빨라질 것이다.  
> 하지만 지금도 충분히 속도가 빠르며 위와같은 방법으로 하면 코드가 좀 지저분해질거같아 일단 이런방식으로 풀어두었다.

```python
def solution(genres, plays):

    import operator

    play_info = dict()
    album = list()

    for i in range(len(genres)):
        if genres[i] in play_info:
            play_info[genres[i]][0] += plays[i]
            play_info[genres[i]][1][i] = plays[i]
        else:
            play_info[genres[i]] = list()
            play_info[genres[i]].insert(0, plays[i])
            play_info[genres[i]].insert(1, {i: plays[i]})

    play_info = sorted(play_info.items(),
                       key=operator.itemgetter(1),
                       reverse=True)

    for genre_info in play_info:
        play_time = genre_info[1][1]
        play_time = sorted(play_time.items(),
                           key=operator.itemgetter(1),
                           reverse=True)

        album.append(play_time[0][0])
        if len(play_time) > 1:
            album.append(play_time[1][0])

    return album
```





## 민성호

> [장르 : 재생 횟수] HashMap을 만든다.  
> 전체 곡을 돌면서 해당 장르의 재생 횟수를 증가시킨다.  
> 정렬한다.
>
> [장르 : 크기2인 정수 배열] HashMap을 만든다  
> 전체 곡을 돌면서 재생 횟수가 조건을 만족하면 value 배열에 넣는다.  
>
> 정렬한 장르순으로, 크기2인 정수 배열에서 0,1 인덱스에 들어 있는 고유 번호를 받는다

```java
import java.util.*;

class Solution{
    public int[] solution(String[] genres, int[] plays){
        // 해시맵에 넣기
        HashMap<String, Integer> map = new HashMap<>();
        for(int i=0; i<genres.length; i++) {
            String genre = genres[i];
            int play = plays[i];
            if(map.get(genre)==null) {
                map.put(genre,play);
            } else {
                map.replace(genre, map.get(genre)+play);
            }
        }

        // 장르 재생 순 정렬
        List<String> list=new ArrayList<>(map.keySet());
        Collections.sort(list,new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                if (map.get(o1)>map.get(o2)) {
                    return -1;
                } else if (map.get(o1)<map.get(o2)) {
                    return 1;
                }
                return 0;
            }
        });

        // map<String, int[2]> 장르를 키로 상위 2개 고유 번호 담기
        HashMap<String, int[]> topMap = new HashMap<>();
        for(int i=0; i<genres.length; i++) {
            int num = i;
            String genre = genres[num];
            int[] topTwo=null;
            if(topMap.containsKey(genre)==false) {
                topTwo = new int[2];
                topTwo[0] = i;
                topTwo[1] = -1;
            } else {
                topTwo = topMap.get(genre);
                if(topTwo[1]==-1) {
                    topTwo[1]=num;
                } else {
                    if (plays[num]>plays[topTwo[1]] || (plays[num]==plays[topTwo[1]] && num<topTwo[1])) {
                        topTwo[1]=num;
                    }
                }
                if (plays[topTwo[1]]>plays[topTwo[0]] || (plays[topTwo[0]]==plays[topTwo[1]] && topTwo[1]<topTwo[0])) {
                    int temp = topTwo[0];
                    topTwo[0] = topTwo[1];
                    topTwo[1] = temp;
                }
            }
            topMap.put(genre,topTwo);
        }

        // 결과를 ArrayList에 넣기
        ArrayList<Integer> ret = new ArrayList<>();
        for(int i=0; i<list.size(); i++) {
            int[] temp = topMap.get(list.get(i));
            for(int j=0; j<2; j++) {
                if(temp[j]!=-1) {ret.add(temp[j]);}
            }
        }

        // ArrayList를 Array로 변환
        int[] result = new int[ret.size()];
        for(int i=0; i<ret.size(); i++) {
            result[i] = ret.get(i);
        }
        return result;
    }
}
```

containsKey 쓸 걸...  
그냥 [장르 : 포함된 곡 인덱스 배열] HashMap 하나 더 만들어서 정렬 할 걸...  
개선된 루프문 쓸 걸...ß