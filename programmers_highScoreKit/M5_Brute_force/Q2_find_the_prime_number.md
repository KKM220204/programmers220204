# 소수 찾기



###### 문제 설명

한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- numbers는 길이 1 이상 7 이하인 문자열입니다.
- numbers는 0~9까지 숫자만으로 이루어져 있습니다.
- "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

##### 입출력 예

| numbers | return |
| ------- | ------ |
| "17"    | 3      |
| "011"   | 2      |

##### 입출력 예 설명

예제 #1
[1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.

예제 #2
[0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다.

- 11과 011은 같은 숫자로 취급합니다.

[출처](http://2009.nwerc.eu/results/nwerc09.pdf)





---------------





## 권지민

> 1. 먼저 연결된 숫자 "17" "011"을 각각의 숫자로 분리한다. [1, 7]   [0, 1, 1]
>
> 2. 각 숫자들로 이어붙여 만들 수 있는 모든 숫자를 ArrayList<Integer> allNum에 넣는다. (전역변수 선언)
>
> 3. allNum에 들어간 숫자 중, 소수인 경우를 count하여 return한다.
>
> 이 때, 2번 과정에서 순열 방식을 사용하는것이 핵심이다.

```java
import java.util.ArrayList;

//numbers에 중복된 숫자들이 들어오면, 순열로 만들 때 값이 중복될 수 있으므로 중복값 삽입을 막아야함
class Solution {
    ArrayList<Integer> allNum = new ArrayList<>();
    //소수 판별
    public boolean isPrime(int num) {
        if (num <= 1)
            return false;
        if (num < 4)
            return true;
        for (int i = 2; i * i <= num; i++) {
            if (num % i == 0)
                return false;
        }
        return true;
    }
   //순열
    public void makeNum(int[] arr, int depth, int n, int r) {
        if(depth == r) {
            String num = "";
            for (int i = 0; i < r; i++)
                num += Integer.toString(arr[i]);
            if (!allNum.contains(Integer.parseInt(num)))
                allNum.add(Integer.parseInt(num));
            return;
        }

        for(int i=depth; i<n; i++) {
            swap(arr, depth, i);
            makeNum(arr, depth + 1, n, r);
            swap(arr, depth, i);
        }
    }
   //두 원소 순서 바꾸기
    public void swap(int[] arr, int depth, int i) {
        int temp = arr[depth];
        arr[depth] = arr[i];
        arr[i] = temp;
    }

    public int solution(String numbers) {
        int answer = 0;
        int []num = new int[numbers.length()];
        
        for (int i = 0; i < numbers.length(); i++) {
            num[i] = numbers.charAt(i) - '0';
        }
        for (int i = 1; i <= num.length; i++) {
            makeNum(num, 0, num.length, i);
        }
        for (int i = 0; i < allNum.size(); i++) {
            //System.out.println(allNum.get(i));
            if (isPrime(allNum.get(i)))
                answer++;
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

> 소수를 판별하는 메서드를 만들고, 순열을 순회할 수 있는 함수를 만든다.   
> 모든 순열(np0,np1,np2,...npn)을 순회하도록 반복문을 돌며, 소수인지 판별한다.  
> 01과 001은 같은 것으로 취급하기 위해서, HashSet을 사용한다

```java
import java.util.HashSet;

class Solution {
    public boolean isPrime(int check){
        if (check==0 || check==1) {
            return false;
        }
        for (int i=2; i<=Math.sqrt(check); i++) {
            if (check%i==0) return false;
        }
        return true;
    } 
    
    public void permutation(String prefix, String str, HashSet<Integer> set) {
        if(!(prefix.equals(""))) {
            set.add(Integer.valueOf(prefix));
        }
        int n = str.length();
        for (int i=0; i<n; i++) {
            permutation(prefix+str.charAt(i), str.substring(0,i)+str.substring(i+1,n), set);
        }
    }
    
    public int solution(String numbers) {
        int cnt = 0;
        HashSet<Integer> set = new HashSet<>();
        
        permutation("", numbers, set);
        
        while (set.iterator().hasNext()) {
            int n = set.iterator().next();
            set.remove(n);
            if(n==2) cnt++;
            if (n%2!=0 && isPrime(n)) {
                cnt++;
            }
        }
        return cnt;
    }
}
```
