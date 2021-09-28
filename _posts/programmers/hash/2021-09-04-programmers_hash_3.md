---
title: 코딩테스트 연습 > 해시 > 위장

author: kimsangtak
date: 2021-09-04 22:00:00 +0800
categories: [programmers,hash]
tags: [programmers,프로그래머스,hash,해쉬,위장]
math: true
mermaid: true

  
---

## 해쉬 문제 3 - 위장
---
만약 이 글을 찾은 분이라면 프로그래머스 해쉬 위장문제를 풀다가 온 사람일 것이므로, 자세한 문제 설명은 생략하겠습니다. 이 문제의 공식 및 풀이 방법 위주로 설명드리겠습니다<br>
 코테 고수처럼 말을 하지만, 사실 공식 찾느라 4시간동안 삽질 후 다른사람 풀이를 보고나서 이해를 하여 포스팅하겠습니다...🤣

<a href="https://programmers.co.kr/learn/courses/30/lessons/42578" target="_blank">문제 링크</a>

## 문제 설명


스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.
예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

| 종류                | 이름                  | 
|---------------------|:---------------------------------:| 
| `얼굴`        | 동그란 안경, 검정 선글라스     |
| `상의`        | 파란색 티셔츠     |
| `하의`        | 청바지     |
| `겉옷`        | 긴 코트     |




스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

## 제한사항

* clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
* 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
* 같은 이름을 가진 의상은 존재하지 않습니다.
* clothes의 모든 원소는 문자열로 이루어져 있습니다.
* 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
* 스파이는 하루에 최소 한 개의 의상은 입습니다.


## 입출력 예

| clothes          | return                  | 
|---------------------|:---------------------------------:| 
| `[["yellowhat", "headgear"], ["bluesunglasses", "eyewear"], ["green_turban", "headgear"]]`    | 5      |


# 입출력 예 설명
예제 #1<br>
headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.

1. yellow_hat
2. blue_sunglasses
3. green_turban
4. yellow_hat + blue_sunglasses
5. green_turban + blue_sunglasses



예제 #2<br>
face에 해당하는 의상이 crow_mask, blue_sunglasses, smoky_makeup이므로 아래와 같이 3개의 조합이 가능합니다.

1. crow_mask
2. blue_sunglasses
3. smoky_makeup



## 풀이

같은 종류의 옷을 제외한 조합 갯수를 찾는 문제.
제한사항 중에 <kbd> 같은 이름을 가진 의상은 존재하지 않습니다.</kbd>란 사항이 있다 즉 종류 별로 묶어도 갯수로만 체크하면 된다는 것이다.

의상의 종류 당 의상의 이름이 몇개가 있는지 파악을 한뒤에  조합법을 도출해내는 특정 공식을 입히면 풀이가 될것이라고 생각했다.    

예제 #1 그림
<img src="/assets/img/programmers/hash/3_1.png" width="90%" height="90%" title="제목" alt="아무거나"/> 

의상의 종류당 의상의 갯수를 파악하는데는 쉬웠지만,  공식을 찾기 위해서 수많은 테스트와 시간이 지나버리곤 학습을 하기위해서란 합리화를 한뒤 다른 사람 풀이를 봐버렸다.
 각 종류의 갯수에 +1 씩 하고 곱한뒤에 마지막 1을 빼라고만 되어있을뿐, 이유를 생각해보니


 아무것도 안입었을 때를 대비해서 종류에 +1를 해준다음 종류끼리 곱한 상태에서 아무것도 입지않은 경우를 빼준 것이 공식이었다.

예제 #1을 설명하자면

[headgear] `장착 X, yellow_hat, green_turban`<br> 
[eyewear] `장착 X , blue_sunglasses` 이므로 모든 조합은


| NO | headgear          | eyewear                  | 
|------|:---------------------|:---------------------------------:| 
|1 |  장착 X  |  장착 X               |     
|2 |  장착 X  |  blue_sunglasses   |
|3 |  yellow_hat |  장착 X         |
|4 |  yellow_hat |  blue_sunglasses  |
|5 |  green_turban | 장착 X          |
|6 |  green_turban | blue_sunglasses |

총 6가지중에서 첫번째의  [headgear] 장착 X  [eyewear] 장착 X 는 제외시켜줘야한다는 것이다
그러므로 공식은 종류마다 +1 해서 곱한뒤 -1 해주는것이다

## 소스 

```java
import java.util.*;

class Solution {
    public int solution(String[][] clothes) {
        int answer = 1;
        HashMap<String, Integer> map = new HashMap<>();

        for(int i=0; i<clothes.length; i++){
            String key = clothes[i][1];
            if(!map.containsKey(key)) {
                map.put(key, 1);
            } else {
                map.put(key, map.get(key) + 1);     //갯수 추가
            }
        }        
        
        for (String key : map.keySet()) {
            int getKey = map.get(key) +1;           //각 종류마다 +1 해서 곱해준다
            answer *= getKey;
        }
        
        return answer-1;                            // 결과에 '모든종류 장착 X 경우' 빼줌
    }
}

```


