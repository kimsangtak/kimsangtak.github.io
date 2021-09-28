---
title: 코딩테스트 연습 > 스택/큐 >  주식가격


author: kimsangtak
date: 2021-09-10 20:48:00 +0800
categories: [programmers,stack]
tags: [programmers,프로그래머스,stack,스택,큐,주식가격, 테스트케이스]
math: true
mermaid: true

  
---

## 스택/큐 문제 4 - 주식가격

---
지문을 읽고나서 뭔 소리인가 했다. 사이트내의 질문하기를 눌러보니 많은 사람들이 혼동이 빚었다
원소마다 뒤의 원소들이 값이 높은 수가 나올때까지의 시간을 계산하는 문제다
정답이 원소마다 다음의 높은 원소를 찾았을 때,  쉼표의 개수로 보면 편하다

<a href="https://programmers.co.kr/learn/courses/30/lessons/42584" target="_blank">문제 링크</a>

## 문제 설명

초 단위로 기록된 주식가격이 담긴 배열 prices가 매개변수로 주어질 때, 가격이 떨어지지 않은 기간은 몇 초인지를 return 하도록 solution 함수를 완성하세요.

## 제한사항

* prices의 각 가격은 1 이상 10,000 이하인 자연수입니다.
* prices의 길이는 2 이상 100,000 이하입니다.

## 입출력 예

|prices	| return	|
|:---------------:|:------:|
|[1, 2, 3, 2, 3]	    |[4, 3, 1, 1, 0]	    |

## 입출력 예 설명

* 1초 시점의 ₩1은 끝까지 가격이 떨어지지 않았습니다.
* 2초 시점의 ₩2은 끝까지 가격이 떨어지지 않았습니다.
* 3초 시점의 ₩3은 1초뒤에 가격이 떨어집니다. 따라서 1초간 가격이 떨어지지 않은 것으로 봅니다.
* 4초 시점의 ₩2은 1초간 가격이 떨어지지 않았습니다.
* 5초 시점의 ₩3은 0초간 가격이 떨어지지 않았습니다.

## 풀이

조건 

1. 리턴의 마지막 값은 무조건 0
2. i,j 순차적으로 비교했을 때 바로 뒤의 원소가 높은 값이라도 시간이 흘렀으므로 0초가 아니라 1초

## 소스 

```java
import java.util.*;

class Solution {
    public int[] solution(int[] prices) {
        int[] answer = {};
        List<Integer> list = new ArrayList<>();
        
        for(int i=0; i<prices.length;i++){
            
            if(prices.length-1 == i){ // 마지막은 무조건 0 
                list.add(0);
                break;
            }
            
            int cnt = 0;
            for(int j=i+1; j<prices.length;j++){
                cnt++;
                if( prices[i] > prices[j]){                  
                    break;
                }
            }
            if(cnt ==0 ){
                cnt =1;   // 0초인 원소는 없다 (0이라면 1초로)
            }
                list.add(cnt);
        }              
        
        answer= new int[list.size()];
        for(int i=0; i<prices.length;i++){
            answer[i] = list.get(i);
        }
        
        return answer;
        
    }
}
```
