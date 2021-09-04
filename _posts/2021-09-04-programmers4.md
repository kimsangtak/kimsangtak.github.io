---
title: 코딩테스트 연습 > 해시 > 베스트앨범

author: kimsangtak
date: 2021-09-04 23:30:00 +0800
categories: [programmers,hash]
tags: [programmers,프로그래머스,hash,해쉬,베스트앨범, 테스트케이스]
math: true
mermaid: true

  
---

## 해쉬 문제 4 - 베스트앨범
---
설명을 잘 이해해야하는 문제...채점 결과 중  5~14번까지 에러나신분들은 잘찾아오신듯 합니다.

<a href="https://programmers.co.kr/learn/courses/30/lessons/42579" target="_blank">문제 링크</a>

## 문제 설명

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

## 제한사항

* genres[i]는 고유번호가 i인 노래의 장르입니다.
* plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
* genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
* 장르 종류는 100개 미만입니다.
* 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
* 모든 장르는 재생된 횟수가 다릅니다.


## 입출력 예

| genres          | plays                  |  return|
|:---------------------:|:---------------------------------:| --:| 
| `["classic", "pop", "classic", "classic", "pop"]`   | `[500, 600, 150, 800, 2500]`	     |[4, 1, 3, 0] |



# 입출력 예 설명

입출력 예 설명
classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.

* 고유 번호 3: 800회 재생
* 고유 번호 0: 500회 재생
* 고유 번호 2: 150회 재생

pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.

* 고유 번호 4: 2,500회 재생
* 고유 번호 1: 600회 재생
* 따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.

※ 공지 - 2019년 2월 28일 테스트케이스가 추가되었습니다.


## 풀이

조건 

1. 각 genres 마다 plays를 합하여 내림차순 정렬 ( 리턴 할 장르의 순서 정하기 )
* 해쉬맵에 장르마다 플레이 수를 구한다음에 sort 통해 내림차순
2. 각 genres안의 plays 1,2위 구하기, 단 genres 안의 plays 가 하나라면 하나만 출력
* 내림차순으로 정렬된 장르 크기만큼 for문 실행해서 genres별 plays 정렬 배열, 정렬되지 않은 plays 배열 생성,  정렬되지않은 plays 고유번호 배열 생성
3. 1위는 무조건 출력, 2위 있으면 출력 
* (만약 1,2위 같으면  정렬되지 않은 plays 배열 ,  정렬되지않은 plays 고유번호 배열 중 1위 삭제하고 2위 찾아 고유번호 출력)



## 테스트 케이스 (테스트케이스 만들기 귀찮으신분들을 위한..)


| genres(string[]) |   plays(int[])						| return                                                                        |
|------------------------------------------------------|---------------------|-----------------------|
| `["classic", "pop", "classic", "classic", "pop"]` 					|  [500, 600, 150, 800, 2500]			|    [4, 1, 3, 0] |
| `["classic", "classic", "classic", "pop"] `							|  [500, 150, 800, 2500]                |   [3, 2, 0] |
| `["classic", "classic", "pop"] `								    	|  	[500, 150, 2500]                    |    [2, 0, 1] |
| `["classic", "classic"] `											    |  	[500, 150]                          |    [0, 1] |
| `["classic", "pop", "classic", "classic", "pop", "cc"]` 			    |  	[1, 2, 3, 5, 5, 8]                  |    [3, 2, 5, 4, 1] |
| `["classic", "pop", "classic", "classic", "pop", "cc", "classic"]`    |  [1, 2, 3, 5, 5, 8, 99]               |   [6, 3, 5, 4, 1] |
| `["a", "b", "a", "a", "b", "c", "a", "c"] `							|  [1, 5, 3, 5, 5, 88, 99, 99]          |   [7, 5, 6, 3, 1, 4] |
| `["a", "b", "a", "a", "b", "c", "a", "c", "c"]` 					    |	[1, 5, 3, 5, 5, 88, 99, 109, 111]   |    [8, 7, 6, 3, 1, 4] |
| `["a", "a", "b", "c", "c", "c"]`								 	    |	[1, 2, 1, 3, 9, 6]                  |    [4, 5, 1, 0, 2] |
| `["a"]`																|[1]                                    |  [0]  |
| `["a", "a", "b", "c", "c", "c", "a", "a", "b", "c", "c", "c"]`		| [1, 2, 1, 3, 9, 6, 1, 2, 1, 3, 9, 6]  |  [4, 10, 1, 7, 2, 8] |

## 소스 

```java
import java.util.*;
//System.out.println(1);
class Solution {
    public static int[] solution(String[] genres, int[] plays) {
      
        int[] answer = {};  
        
        //장르 순서 정하기 위해 해쉬맵으로 (장르,장르별 플레이 수의 합) 저장
        HashMap<String,Integer> hm = new HashMap<String,Integer>();     
        for(int i =0 ; i<genres.length; i++){            
            hm.put( genres[i], hm.getOrDefault( genres[i],0) + plays[i] );       
        }   
        List<String> keySetList = new ArrayList<>(hm.keySet());	
        //keySetList에 해쉬맵 key 별 내림차순으로 저장
		Collections.sort(keySetList, (o1, o2) -> (hm.get(o2).compareTo(hm.get(o1))));
	        
        List<Integer> playList = new ArrayList<>();		// 정렬 할 리스트
        List<Integer> playList2 = new ArrayList<>();	// 고유번호 리스트
        List<Integer> playList3 = new ArrayList<>();	// 정렬 하기전 리스트
        List<Integer> result = new ArrayList<>();	
        
        int first ;  //장르 내 첫번째로 제일 큰 plays값
        int second ; //장르 내 두번째로 제일 큰 plays값
        for(int i =0 ; i<keySetList.size(); i++){               
            for(int j =0 ; j<genres.length; j++){
                if(keySetList.get(i).equals(genres[j])){
                    playList.add(plays[j]);                    
                    playList2.add(j);  
                }
            }  
            playList3.addAll(playList);  // 아직 정렬되지않은 playList를 playList3에 복사
         
            // playList를 내림차순으로 정렬               
            Collections.sort(playList, Collections.reverseOrder());
            first =playList2.get( playList3.indexOf( playList.get(0) ) ) ;
            
            // 첫번째는 무조건 추출
            result.add( first ); 
            if(playList.size()>1){  
                second =playList2.get( playList3.indexOf( playList.get(1)) ) ;       
                
                // 첫번째와 두번째가 같다면 playList2,playList3 에서 첫번째 원소를 삭제한다
                if(first==second){
                    int a = playList.get(0);  //정렬된 plays에서 제일 큰 값
                    int b = playList3.indexOf(a); // a 가 정렬되지않은 곳에서 몇번째 원소인지 확인
                    playList3.remove(b);  // 정렬되지 않은 plays 원소제거                    
                    playList2.remove(b);  // 정렬되지 않은 고유번호 원소제거
                    second =playList2.get( playList3.indexOf( playList.get(1)) ) ;                
                }
                result.add(second);
            }
            
            //플레이리스트들 초기화
            playList = new ArrayList<>();	 	
            playList2 = new ArrayList<>();	
            playList3 = new ArrayList<>();	            
		}                 
        
        //리턴 할 배열 추출
        answer = new int[result.size()];
        for(int i =0 ; i<result.size(); i++){
         answer[i] =   result.get(i);
        }
          
        return answer;
    }
    
}

```

처음에 문제를 genres 의 plays 총합의 top2 genres 정렬을 구한 뒤, 각각의 genres에서 top2의 정렬을 구하는 것이라고 잘못 이해하여 굉장히 쉬운 문제라고 생각했는데,
알고보니 장르의 종류가 여러개이며 장르마다 top2 plays 고유번호를 출력해야 했던것이다. <kbd>  5~14번 문제 틀린 이유 </kbd>  

어찌저찌 풀고나서 다른사람의 답을 보니 나는 주먹구구식으로 푼것 같고, 더 객체지향적인 다른분들의 답은 생성자를 통해 객체를 생성해서 푸는것이 더 직관적이고 변경이 쉬운 답인것 같다.

```java

import java.util.*;

public class Solution {

    static class Music{
        String genre;
        int play;
        int idx;

        public Music(String genre, int play, int idx) {
            this.genre = genre;
            this.play = play;
            this.idx = idx;
        }
    }

    public static int[] solution(String[] genres, int[] plays) {

        HashMap<String, Integer> map = new HashMap<>();
        for(int i=0; i<genres.length; i++){
            map.put(genres[i], map.getOrDefault(genres[i], 0)+plays[i]);
        }

        // 1. 장르 선정
        ArrayList<String> genres_ordered = new ArrayList<>();
        while(map.size()!=0){
            int max = -1;
            String max_key = "";
            for(String key : map.keySet()){
                int tmp_cnt = map.get(key);
                if(tmp_cnt>max){
                    max = tmp_cnt;
                    max_key = key;
                }
            }
            genres_ordered.add(max_key);
            map.remove(max_key);
        }

        // 2. 장르 내 노래 선정
        ArrayList<Music> result = new ArrayList<>();
        for(String gern : genres_ordered){
            ArrayList<Music> list = new ArrayList<>();
            for(int i=0; i<genres.length; i++){
                if(genres[i].equals(gern)){
                    list.add(new Music(gern, plays[i], i));
                }
            }
            Collections.sort(list, (o1, o2) -> o2.play - o1.play); // 내림차순 소팅
            result.add(list.get(0));    // 1개는 무조건 수록
            if(list.size()!=1){     // 더 수록할 곡이 있으면(==장르 내의 노래가 1개보다 많으면) 수록
                result.add(list.get(1));
            }
        }

        // print result
        int[] answer = new int[result.size()];
        for(int i=0; i<result.size(); i++){
            answer[i] = result.get(i).idx;
        }
        return answer;
    }
}
```

