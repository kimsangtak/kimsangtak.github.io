---
title: 코딩테스트 연습 > 스택/큐 > 기능개발

author: kimsangtak
date: 2021-09-08 17:48:00 +0800
categories: [programmers,stack]
tags: [programmers,프로그래머스,stack,스택,큐,기능개발, 테스트케이스]
math: true
mermaid: true

  
---

## 스택/큐 문제 1 - 기능개발
---
스택 메소드를 사용하지않고(문제 풀 때 스택 자체를 잘 몰라서..) arrayList로 코드를 짜보았습니다. 이후 스택 개념을 찾아봐 개념정리를 하였습니다.

<a href="https://programmers.co.kr/learn/courses/30/lessons/42586" target="_blank">문제 링크</a>

## 문제 설명

프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

## 제한사항

* 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
* 작업 진도는 100 미만의 자연수입니다.
* 작업 속도는 100 이하의 자연수입니다.
* 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.


## 입출력 예

| progresses                | speeds                  |     return       |
|:---------------------:|:---------------------------------:| :--:| 
| [93, 30, 55]        | [1, 30, 5]     |	[2, 1] |
| [95, 90, 99, 99, 80, 99]        | [1, 1, 1, 1, 1, 1]     |	[1, 3, 2]|



# 입출력 예 설명
입출력 예 설명

입출력 예 #1

첫 번째 기능은 93% 완료되어 있고 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포가 가능합니다.<br>

두 번째 기능은 30%가 완료되어 있고 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포가 가능합니다. 
하지만 이전 첫 번째 기능이 아직 완성된 상태가 아니기 때문에 첫 번째 기능이 배포되는 7일째 배포됩니다.

세 번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능합니다.
따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포됩니다.

입출력 예 #2

모든 기능이 하루에 1%씩 작업이 가능하므로, 작업이 끝나기까지 남은 일수는 각각 5일, 10일, 1일, 1일, 20일, 1일입니다. 어떤 기능이 먼저 완성되었더라도 앞에 있는 모든 기능이 완성되지 않으면 배포가 불가능합니다.

따라서 5일째에 1개의 기능, 10일째에 3개의 기능, 20일째에 2개의 기능이 배포됩니다.



## 풀이

조건 

1. 각 작업마다 몇일이 지나야 배포 가능한지 구한다
* 예제 1로 적용하자면 7,3,9
2. 배포가능 일자의 분기점을 정한다 
*  7 3  / 9
3. 분기점 배열에서 차이를 구한다
*  결과로 [2, 1] 이 나와야하므로 시작날짜인 0 을 배열에 첫번째요소에 삽입 마지막 요소는 [배열 길이 - 나머지요소들의 합]




## 소스 

```java
import java.util.*;

class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        int[] answer = {};
        List<Integer> distribute_arr = new ArrayList<Integer>();   //얼마 뒤에 배포 가능한지 배열
        List<Integer> subtract_arr = new ArrayList<Integer>();  //날짜마다 차이 배열
        List<Integer> date_length_arr = new ArrayList<Integer>();  //날짜 길이 배열

        for(int i=0; i<progresses.length; i++ ){
            int a = (100 - progresses[i]) /speeds[i];
            int b =  (100 - progresses[i]) % speeds[i] ;
            if(b>0){
                a++;
            }
            distribute_arr.add(a);
        }
        if(distribute_arr.size()==1){
            answer=new int[1];
            answer[0] = distribute_arr.get(0); 
            return answer;
        }
        int cnt =0;
        subtract_arr.add(0); // 처음에는 시작날짜인 0을  추가
        int temp =distribute_arr.get(0);
        for(int i=1; i<distribute_arr.size() ; i++ ){
            if(temp >=distribute_arr.get(i)){
                continue;
            }else{
            temp = distribute_arr.get(i);
            subtract_arr.add(i);
            }
        } 
      
        for(int i=1; i<subtract_arr.size() ; i++ ){
                  
          date_length_arr.add(  subtract_arr.get(i) - subtract_arr.get(i-1)   ); 
        }
        cnt =0;
        for(int i=0; i<date_length_arr.size() ; i++ ){
           cnt += date_length_arr.get(i);
        }
        if(cnt != progresses.length){
         date_length_arr.add(progresses.length - cnt);
        }

        answer=new int[date_length_arr.size()];
        for(int i=0; i<date_length_arr.size() ; i++ ){
            answer[i] = date_length_arr.get(i);
        }
        return answer;
    }
}
```
