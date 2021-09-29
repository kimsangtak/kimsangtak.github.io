---
title: 코딩테스트 연습 > 힙 > 디스크 컨트롤러


author: kimsangtak
date: 2021-09-13 23:30:00 +0800
categories: [programmers,heap]
tags: [programmers,프로그래머스,heap,힙,우선순위,우선순위큐,디스크 컨트롤러,테스트케이스]
math: true
mermaid: true

  
---

## 힙 문제 2 - 디스크 컨트롤러

---
개어렵다... 1번문제를 쉽게풀어서 이문제는 이틀간 풀다가 결국 답을 봐버렸다...  다시 한번 풀어봐야지

<a href="https://programmers.co.kr/learn/courses/30/lessons/42627" target="_blank">문제 링크</a>

## 문제 설명

하드디스크는 한 번에 하나의 작업만 수행할 수 있습니다. 디스크 컨트롤러를 구현하는 방법은 여러 가지가 있습니다. 가장 일반적인 방법은 요청이 들어온 순서대로 처리하는 것입니다.

- 0ms 시점에 3ms가 소요되는 A작업 요청
- 1ms 시점에 9ms가 소요되는 B작업 요청
- 2ms 시점에 6ms가 소요되는 C작업 요청
한 번에 하나의 요청만을 수행할 수 있기 때문에 각각의 작업을 요청받은 순서대로 처리하면 다음과 같이 처리 됩니다.

- A: 3ms 시점에 작업 완료 (요청에서 종료까지 : 3ms)
- B: 1ms부터 대기하다가, 3ms 시점에 작업을 시작해서 12ms 시점에 작업 완료(요청에서 종료까지 : 11ms)
- C: 2ms부터 대기하다가, 12ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 16ms)
이 때 각 작업의 요청부터 종료까지 걸린 시간의 평균은 10ms(= (3 + 11 + 16) / 3)가 됩니다.

하지만 A → C → B 순서대로 처리하면
- A: 3ms 시점에 작업 완료(요청에서 종료까지 : 3ms)
- C: 2ms부터 대기하다가, 3ms 시점에 작업을 시작해서 9ms 시점에 작업 완료(요청에서 종료까지 : 7ms)
- B: 1ms부터 대기하다가, 9ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 17ms)
이렇게 A → C → B의 순서로 처리하면 각 작업의 요청부터 종료까지 걸린 시간의 평균은 9ms(= (3 + 7 + 17) / 3)가 됩니다.

각 작업에 대해 [작업이 요청되는 시점, 작업의 소요시간]을 담은 2차원 배열 jobs가 매개변수로 주어질 때, 작업의 요청부터 종료까지 걸린 시간의 평균을 가장 줄이는 방법으로 처리하면 평균이 얼마가 되는지 return 하도록 solution 함수를 작성해주세요. (단, 소수점 이하의 수는 버립니다)

## 제한사항
- jobs의 길이는 1 이상 500 이하입니다.
- jobs의 각 행은 하나의 작업에 대한 [작업이 요청되는 시점, 작업의 소요시간] 입니다.
- 각 작업에 대해 작업이 요청되는 시간은 0 이상 1,000 이하입니다.
- 각 작업에 대해 작업의 소요시간은 1 이상 1,000 이하입니다.
- 하드디스크가 작업을 수행하고 있지 않을 때에는 먼저 요청이 들어온 작업부터 처리합니다.

## 입출력 예

| jobs          |  return|
|:-----------:|:--------------------:|
| `[[0, 3], [1, 9], [2, 6]]	`  | `9` |



# 입출력 예 설명

문제에 주어진 예와 같습니다.

- 0ms 시점에 3ms 걸리는 작업 요청이 들어옵니다.
- 1ms 시점에 9ms 걸리는 작업 요청이 들어옵니다.
- 2ms 시점에 6ms 걸리는 작업 요청이 들어옵니다.

## 풀이

조건 

1.  2개의 우선순위 큐 선언 
    * 첫번째 대기큐-  요청시간이 적은 순대로 정렬, 두번째 작업큐 = 소요시간 적은 순
2. 1초마다 while문 반복 - 작업큐에 대기열이 비어있지 않고 요청 시점이 되었을 때 추가
3. 작업 큐에 작업이 있으면 우선순위에 따라 작업 진행
4. 작업이 없으면 시간 1ms 흐름

## 소스 

```java
import java.util.*;

class Solution {
    class Job {
        int reqTime;
        int runTime;
        
        public Job(int req, int run){
            this.reqTime = req;
            this.runTime = run;
        }
    }
    
    public int solution(int[][] jobs) {
        // 대기열 → '작업이 요청되는 시점'이 빠른 순으로 정렬되는 우선순위 큐
        PriorityQueue<Job> q = new PriorityQueue(new Comparator<Job>() {
            @Override
            public int compare(Job j1, Job j2){
                return j1.reqTime - j2.reqTime;
            }
        });
        for(int[] job : jobs){
            q.offer(new Job(job[0],job[1]));
        }
        
        // 작업 큐 → '작업이 소요시간'이 적은 순으로 정렬되는 우선순위 큐
        PriorityQueue<Job> pq = new PriorityQueue(new Comparator<Job>() {
            @Override
            public int compare(Job j1, Job j2){
                return j1.runTime - j2.runTime;
            }
        });
        
        int count = 0;
        int sum = 0;
        int time = 0;
        // 모든 작업이 끝날때까지 while문 반복
        while(count < jobs.length){
            // 1ms마다 while문 반복
            // 작업 큐에 추가 (대기열이 비어있지 않고 요청 시점이 되었을 때 추가)
            while(!q.isEmpty() && time >= q.peek().reqTime){
                pq.offer(q.poll());
            }
            // 작업 큐에 작업이 있으면 우선순위에 따라 작업 진행
            if(!pq.isEmpty()){
                Job j = pq.poll();
                sum += j.runTime + (time - j.reqTime);
                time += j.runTime;
                count++;
            }
            else {  // 작업이 없으면 시간 1ms 흐름
                time++;
            }     
            System.out.println(time);

        }
        // 작업의 요청부터 종료까지 걸린 시간의 평균 리턴
        return sum / count;
    }
}
```

