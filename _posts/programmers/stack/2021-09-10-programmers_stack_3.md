---
title: 코딩테스트 연습 > 스택/큐 > 다리를 지나는 트럭


author: kimsangtak
date: 2021-09-10 20:48:00 +0800
categories: [programmers,stack]
tags: [programmers,프로그래머스,stack,스택,큐,다리를 지나는 트럭
, 테스트케이스]
math: true
mermaid: true

  
---

## 스택/큐 문제 3 - 다리를 지나는 트럭

---
큐 메소드를 사용하여 객체에 저장 후 푸는 문제. 이번 기회에 큐 메소드를 이해하고 적용해서 문제를 풀었다. 그동안 arraylist만 사용했는데
후입선출의 경우 큐를 쓰면 굉장히 유용하다

<a href="https://programmers.co.kr/learn/courses/30/lessons/42583" target="_blank">문제 링크</a>

## 문제 설명

트럭 여러 대가 강을 가로지르는 일차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며, 다리는 weight 이하까지의 무게를 견딜 수 있습니다. 단, 다리에 완전히 오르지 않은 트럭의 무게는 무시합니다.

예를 들어, 트럭 2대가 올라갈 수 있고 무게를 10kg까지 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

|경과 시간	| 다리를 지난 트럭	| 다리를 건너는 트럭	| 대기 트럭|
|:--------:|:----------------:|:--------------------:| :-----:| 
| 0		| []	       | []    |    [7,4,5,6]       |
| 1~2	| []	       | [7]   |   	[4,5,6]     |
| 3	    | [7]	       | [4]   |   [5,6]          |
| 4	    | [7]	       | [4,5] |  [6]         |
| 5	    | [7,4]	       | [5]   |   [6]            |
| 6~7   | [7,4,5]      | [6]   |   []          |
| 8 	| [7,4,5,6]    | []	   |   []             |

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리에 올라갈 수 있는 트럭 수 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭 별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

## 제한사항
* bridge_length는 1 이상 10,000 이하입니다.
* weight는 1 이상 10,000 이하입니다.
* truck_weights의 길이는 1 이상 10,000 이하입니다.
* 모든 트럭의 무게는 1 이상 weight 이하입니다.

## 입출력 예



|bridge_length	| weight	| truck_weights	| return|
|:---------------:|:------:|:----------------------:| :--:| 
|2	    |10	    |[7,4,5,6]	                        |8      |
|100	|100	|[10]	                            |101    |
|100	|100	|[10,10,10,10,10,10,10,10,10,10]	|110    |

## 풀이

조건 

1. 1번큐 : 다리건너기 전 대기중인 트럭 큐 ,2번큐 : 다리위의 트럭무게 큐 생성

2. 다리위의 트럭무게 큐에 트럭이 없으면 0으로 채워넣기

3. 초마다 상황을 반복문 돌린다
* 초마다 다리위의 적재되어 있는 무게 구한다
* 구한 무게 + 다리건너기전 대기중인 첫번째 트럭 무게가 제한 무게보다 적으면  1번큐의 첫번째를 2번큐에 적재시킨다
4. 대기중인 트럭이 없고 다리 적재되어있는 트럭무게의 합이 0이면 종료




## 소스 

```java
import java.util.*;

class Solution {
    public int solution(int bridge_length, int weight, int[] truck_weights) {
        
		int answer = 0;
        Queue<Integer> wait_que = new LinkedList<Integer>();     //다리 건너기 전 대기중인 트럭 큐
        Queue<Integer> bridge_que = new LinkedList<Integer>();   //다리 각각의 적재되어있는 트럭 무게

        for (int i : truck_weights) {
            wait_que.add(i);
        }
        for (int i =0;i<bridge_length;i++){
            bridge_que.add(0);  // 초기화 시 다리위에 아무것도 없으므로 0 주입
        }

        while(true){
            int bridge_sum=0;
            answer++;
			bridge_que.poll();

            Iterator iter = bridge_que.iterator();            
            while(iter.hasNext()){
                bridge_sum += (int)iter.next();  // 다리위에 적재되어 있는 무게 구한다
            }
            int hap = bridge_sum + Int( wait_que.peek() ) ; // 다리건너기전 대기중인 첫번째 트럭 무게 + 다리 위 적재무게 합
            if( hap <=weight){               
                bridge_que.add(Int(wait_que.poll()));  // 대기중인 트럭을 다리에 올린다
            }else{
                bridge_que.add(0); // 초 마다 진행 되므로 아무 트럭이 올라오지 않으면 0 추가
            }     

            if(wait_que.size()==0 ){
                if(hap==0 ){ // 대기중인 트럭이 없고 다리 적재되어있는 트럭무게의 합이 0이면 종료
                    break;
                }
            }

        }
         
        return answer;
    }

    public static int  Int(Object obj) {
        if( obj == null ) return 0;
        else{           
            try{            
                return (Integer) obj;               
            }catch (java.lang.ClassCastException e) {               
                try{                    
                    return Integer.parseInt( obj.toString() );                  
                }catch( Exception ex ){                 
                    return -1;                  
                }               
            }           
        }
    }
}
```
