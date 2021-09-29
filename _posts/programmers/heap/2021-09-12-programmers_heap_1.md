---
title: 코딩테스트 연습 > 힙 > 더맵게

author: kimsangtak
date: 2021-09-12 23:30:00 +0800
categories: [programmers,heap]
tags: [programmers,프로그래머스,heap,힙,우선순위,우선순위큐,더맵게,테스트케이스]
math: true
mermaid: true

  
---

## 힙 문제 1 - 더맵게
---
힙이라는 자료구조를 구현하기 위해 우선순위큐를 사용하여 효율적으로 코드작성이 가능하다. 큐에 넣음에 동시에 정렬을 하고싶다면 우선순위큐를 사용한다.

<a href="https://programmers.co.kr/learn/courses/30/lessons/42626" target="_blank">문제 링크</a>

## 문제 설명

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

 <kbd>섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)</kbd>

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

## 제한사항
* scoville의 길이는 2 이상 1,000,000 이하입니다.
* K는 0 이상 1,000,000,000 이하입니다.
* scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
* 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.


## 입출력 예

| scoville          | K                  |  return|
|:---------------------:|:---------------------------------:| --:| 
| `[1, 2, 3, 9, 10, 12]`  | `7` |`2` |



# 입출력 예 설명

1. 스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
새로운 음식의 스코빌 지수 = 1 + (2 * 2) = 5
가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]

2. 스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
새로운 음식의 스코빌 지수 = 3 + (5 * 2) = 13
가진 음식의 스코빌 지수 = [13, 9, 10, 12]

모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.


## 풀이

조건 

1. 배열을 우선순위 큐에 오름차순으로 정렬
2. poll 한  2개의 원소를를  공식을 사용하여  큐에 넣음
3. k보다 크면 종료 카운트 리턴

## 소스 

```java

import java.util.*;

class Solution {
    public int solution(int[] scoville, int K){
        int allMixcase = scoville.length-1;
        int mixScoville = 0;
        
        PriorityQueue<Integer> tempScoville = new PriorityQueue<>();        
        
        for(int i = 0 ; i<scoville.length;i++){
            tempScoville.add(scoville[i]);   // 우선순위 큐에 오름차순으로 데이터 주입
        }
        
        if(tempScoville.peek() >= K) return 0; // 최소가 k보다 클 경우
        
        for(int cnt = 0; cnt < allMixcase; cnt++)        {
            mixScoville = tempScoville.poll()+(tempScoville.poll()*2);        
            tempScoville.add(mixScoville); 

            if(tempScoville.peek() >= K) return cnt+1;  // 종료
        }
        return -1;
    }
}
```

