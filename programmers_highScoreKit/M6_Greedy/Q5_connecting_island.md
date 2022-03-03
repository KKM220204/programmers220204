# 섬 연결하기



###### 문제 설명

n개의 섬 사이에 다리를 건설하는 비용(costs)이 주어질 때, 최소의 비용으로 모든 섬이 서로 통행 가능하도록 만들 때 필요한 최소 비용을 return 하도록 solution을 완성하세요.

다리를 여러 번 건너더라도, 도달할 수만 있으면 통행 가능하다고 봅니다. 예를 들어 A 섬과 B 섬 사이에 다리가 있고, B 섬과 C 섬 사이에 다리가 있으면 A 섬과 C 섬은 서로 통행 가능합니다.

**제한사항**

- 섬의 개수 n은 1 이상 100 이하입니다.
- costs의 길이는 `((n-1) * n) / 2`이하입니다.
- 임의의 i에 대해, costs[i][0] 와 costs[i] [1]에는 다리가 연결되는 두 섬의 번호가 들어있고, costs[i] [2]에는 이 두 섬을 연결하는 다리를 건설할 때 드는 비용입니다.
- 같은 연결은 두 번 주어지지 않습니다. 또한 순서가 바뀌더라도 같은 연결로 봅니다. 즉 0과 1 사이를 연결하는 비용이 주어졌을 때, 1과 0의 비용이 주어지지 않습니다.
- 모든 섬 사이의 다리 건설 비용이 주어지지 않습니다. 이 경우, 두 섬 사이의 건설이 불가능한 것으로 봅니다.
- 연결할 수 없는 섬은 주어지지 않습니다.

**입출력 예**

| n    | costs                                     | return |
| ---- | ----------------------------------------- | ------ |
| 4    | [[0,1,1],[0,2,2],[1,2,5],[1,3,1],[2,3,8]] | 4      |

**입출력 예 설명**

costs를 그림으로 표현하면 다음과 같으며, 이때 초록색 경로로 연결하는 것이 가장 적은 비용으로 모두를 통행할 수 있도록 만드는 방법입니다.

![image.png](https://grepp-programmers.s3.amazonaws.com/files/production/13e2952057/f2746a8c-527c-4451-9a73-42129911fe17.png)





---------------





## 권지민

> 크루스칼 알고리즘 필요 (서로소).  
> 건설 비용이 낮은 순으로 정렬한 후, 간선 연결을 시작한다.  
> 각 노드의 부모가 일치하지 않으면 채택, 일치하면 pass

```java
import java.util.Arrays;
import java.util.ArrayList;
import java.util.Comparator;
class Solution {
    public int findParent(int[] parent, int node) {
        if (parent[node] == node)
            return node;
        else
            return findParent(parent, parent[node]);
    }
    public void union(int[] parent, int node1, int node2) {
        int p1 = findParent(parent, node1);
        int p2 = findParent(parent, node2);
        
        if (p1 < p2)
            parent[p2] = p1;
        else
            parent[p1] = p2;
    } 
    public int solution(int n, int[][] costs) {
        int answer = 0;
        int []parent = new int[Math.max(n, costs.length)]; // n : 2 costs[0, 1, 1]인경우 대비
        
        for (int i = 0; i < parent.length; i++)
            parent[i] = i;
        Arrays.sort(costs, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[2] - o2[2];
            }
        });
        for (int i = 0; i < costs.length; i++) {
            if(findParent(parent, costs[i][0]) != findParent(parent, costs[i][1])) { // 사이클 판단
                answer += costs[i][2];
                union(parent, costs[i][0], costs[i][1]);
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

> 크루스칼 알고리즘 사용

```java
import java.util.*;

class Solution {
    static int[] parent;
    
    
    public int solution(int n, int[][] costs) {
        Arrays.sort(costs, (int[] c1, int[] c2)->c1[2]-c2[2]);
        parent = new int[n];
        
        for(int i=0; i<n; i++){
            parent[i] = i;
        }
        
        int total = 0;
        for(int[] edge: costs) {
            int from = edge[0];
            int to = edge[1];
            int cost = edge[2];
            
            int fromParent = findParent(from);
            int toParent = findParent(to);
            
            if(fromParent == toParent) continue;
            
            total += cost;
            parent[toParent] = fromParent;
        }
        return total;
    }
    
    private int findParent(int node) {
        if(parent[node] == node) return node;
        parent[node] = findParent(parent[node]);
        return parent[node];
    }
    
}
```
