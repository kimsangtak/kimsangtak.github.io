---
title: 코딩테스트 연습 > 스택/큐 > 프린터

author: kimsangtak
date: 2021-09-09 20:48:00 +0800
categories: [programmers,stack]
tags: [programmers,프로그래머스,stack,스택,큐,프린터, 테스트케이스]
math: true
mermaid: true

  
---

## 스택/큐 문제 2 - 프린터
---
큐 메소드를 사용하여 객체에 저장 후 푸는 문제. 풀고나서 보니 큐를 사용하여 풀면 더 깔끔하지 않았나 싶다. 맨 앞의 요소를 빼고 맨 뒤에 요소에 넣는 형식에  큐 메소드를 사용하는게 더 바람직하다.

<a href="https://programmers.co.kr/learn/courses/30/lessons/42587" target="_blank">문제 링크</a>

## 문제 설명

일반적인 프린터는 인쇄 요청이 들어온 순서대로 인쇄합니다. 그렇기 때문에 중요한 문서가 나중에 인쇄될 수 있습니다. 이런 문제를 보완하기 위해 중요도가 높은 문서를 먼저 인쇄하는 프린터를 개발했습니다. 이 새롭게 개발한 프린터는 아래와 같은 방식으로 인쇄 작업을 수행합니다.

1. 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.
2. 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.
3. 그렇지 않으면 J를 인쇄합니다.

예를 들어, 4개의 문서(A, B, C, D)가 순서대로 인쇄 대기목록에 있고 중요도가 2 1 3 2 라면 C D A B 순으로 인쇄하게 됩니다.

내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 알고 싶습니다. 위의 예에서 C는 1번째로, A는 3번째로 인쇄됩니다.

현재 대기목록에 있는 문서의 중요도가 순서대로 담긴 배열 priorities와 내가 인쇄를 요청한 문서가 현재 대기목록의 어떤 위치에 있는지를 알려주는 location이 매개변수로 주어질 때, 내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 return 하도록 solution 함수를 작성해주세요.

## 제한사항

* 현재 대기목록에는 1개 이상 100개 이하의 문서가 있습니다.
* 인쇄 작업의 중요도는 1~9로 표현하며 숫자가 클수록 중요하다는 뜻입니다.
* location은 0 이상 (현재 대기목록에 있는 작업 수 - 1) 이하의 값을 가지며 대기목록의 가장 앞에 있으면 0, 두 번째에 있으면 1로 표현합니다.

## 입출력 예

| progresses                | speeds                  |     return       |
|:---------------------:|:---------------------------------:| :--:| 
| [2, 1, 3, 2]      | 2    |	1|
|[1, 1, 9, 1, 1, 1]	      | 0     |	5|



# 입출력 예 설명
입출력 예 설명

예제 #1

문제에 나온 예와 같습니다.

예제 #2

6개의 문서(A, B, C, D, E, F)가 인쇄 대기목록에 있고 중요도가 1 1 9 1 1 1 이므로 C D E F A B 순으로 인쇄합니다.



## 풀이

조건 

1. 객체 생성 (idx, priories, bools) bools은 로케이션 확인여부값

2. 객체에 반복 횟수값과 location값을 비교해 객체에 저장

3. 반복문을 통해  뒤에있는 값보다 큰게 있다면 결과 리스트에 저장  

4. 결과리스트를 통해서 객체의 bools 값 까지 횟수 도출




## 소스 

```java
import java.util.*;
class Solution {
    public class setList {
        int idx;
        int prior;
        boolean bools;
        public setList(int idx, int prior,boolean bools) {
            this.idx = idx;
            this.prior = prior;
            this.bools = bools;
        }
    }

    public int solution(int[] priorities, int location) {
        
		int answer=0;
        List<setList> list = new ArrayList<>();
        List<setList> result_list = new ArrayList<>(); // 정렬된 리스트
        for (int i = 0; i < priorities.length; i++) {
            if(i==location){
                list.add(new setList(i, priorities[i],true));
            }else{
            list.add(new setList(i, priorities[i],false));    
            }

        }
        boolean bool = false;
        boolean prior_bool = false;  // 우선순위 확인 불린
        int set0 = 0;
        while (bool == false) {
            set0 = list.get(0).prior;
            for (int i = 1; i < list.size(); i++) {
                if (set0 < list.get(i).prior) {
                    prior_bool = true;
                    break;
                }
            }
            if (prior_bool == true) {
                list.add(list.get(0));
                list.remove(0);
                prior_bool =false ;
            } else {
                result_list.add(list.get(0));
                list.remove(0);
            }
            if(list.size()==0) {
                bool=true;
            }
        }

        for (int i = 0; i < result_list.size(); i++) {
            if( result_list.get(i).bools ==true){


            answer = i+1;    
            }
        }

return answer;
    }
}
```
