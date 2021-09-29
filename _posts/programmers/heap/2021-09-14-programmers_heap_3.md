---
title: 코딩테스트 연습 > 힙 > 이중우선순위큐


author: kimsangtak
date: 2021-09-14 23:30:00 +0800
categories: [programmers,heap]
tags: [programmers,프로그래머스,heap,힙,우선순위,우선순위큐,이중우선순위큐,테스트케이스]
math: true
mermaid: true

  
---

## 힙 문제 2 - 이중우선순위큐

---
우선순위 큐를 사용해야 하는 문제인데 앞의 요소와 뒤의 요소를 가공할수있는 LinkedList를 활용하여 문제를 풀었다.


<a href="https://programmers.co.kr/learn/courses/30/lessons/42628" target="_blank">문제 링크</a>

## 문제 설명

이중 우선순위 큐는 다음 연산을 할 수 있는 자료구조를 말합니다.

| 명령어          |  수신 탑(높이) |
|:-----------:|:--------------------:|
| `I 숫자`  | `큐에 주어진 숫자를 삽입합니다.` |
| `D 1`  | `큐에서 최댓값을 삭제합니다.` |
| `D -1`  | `큐에서 최솟값을 삭제합니다.` |

이중 우선순위 큐가 할 연산 operations가 매개변수로 주어질 때, 모든 연산을 처리한 후 큐가 비어있으면 [0,0] 비어있지 않으면 [최댓값, 최솟값]을 return 하도록 solution 함수를 구현해주세요.


## 제한사항

* operations는 길이가 1 이상 1,000,000 이하인 문자열 배열입니다.
* operations의 원소는 큐가 수행할 연산을 나타냅니다.
    * 원소는 “명령어 데이터” 형식으로 주어집니다.- 최댓값/최솟값을 삭제하는 연산에서 최댓값/최솟값이 둘 이상인 경우, 하나만 삭제합니다.
        
* 빈 큐에 데이터를 삭제하라는 연산이 주어질 경우, 해당 연산은 무시합니다.

## 입출력 예

| operations          |  return|
|:-----------:|:--------------------:|
|`["I 16","D 1"]`	| `[0,0]` | 
|`["I 7","I 5","I -5","D -1"]` | 	`[7,5]` |


# 입출력 예 설명
16을 삽입 후 최댓값을 삭제합니다. 비어있으므로 [0,0]을 반환합니다.<br />
7,5,-5를 삽입 후 최솟값을 삭제합니다. 최대값 7, 최소값 5를 반환합니다.

## 풀이

조건 

1. 반복문 시작 시 정렬 진행
2. 삽입문일때 숫자만 링크드리스트에 삽입
3. 최대값 삭제일 시 마지막 요소 삭제 최소값 삭제일 시 첫번째 요소 삭제
4. 반복문의 끝난뒤 sort 한번 더 진행

## 소스 

```java
import java.util.*;

class Solution {
    public int[] solution(String[] operations) {
        int[] answer = new int[2];
        LinkedList<Integer> ll = new LinkedList<Integer>();
        for(int i=0; i< operations.length; i++){

            if(ll.size()>0){
                Collections.sort(ll);
            }
                    
			//추가  
            if( operations[i].startsWith("I")){
                int num = Integer.parseInt (operations[i].replaceAll("[^0-9-]","")) ;
                ll.add(num);
                
            }else{
                if(ll.size()==0){
                    continue;
                }
                
                // 최대값 삭제
                if( operations[i].indexOf("-") < 0){
                    ll.removeLast();                 
                    
                // 최소값 삭제
                }else{
                    ll.removeFirst();
                }
                
            }
        }            
        
        if(ll.size()>0){   // 마지막에 정렬 한번 더
            Collections.sort(ll);
        }
        
        if(ll.size()==0){
            answer[0]=0;
            answer[1]=0;
        }else{
            answer[0]=ll.getLast();
            answer[1]=ll.getFirst();
        }
        return answer;
    }
}
```

