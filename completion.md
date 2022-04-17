## 🏃🏻 완주하지 못한 선수

#### 문제 설명
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

#### 제한사항
- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.

- completion의 길이는 participant의 길이보다 1 작습니다.

- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.

- 참가자 중에는 동명이인이 있을 수 있습니다.

#### 입출력 예
| participant  | completion	|  return   |
|---|---|---|
| ["leo", "kiki", "eden"]		|  ["eden", "kiki"] | "leo"  |
| ["marina", "josipa", "nikola", "vinko", "filipa"]  | ["josipa", "filipa", "marina", "nikola"]  | "vinko"  |
| ["mislav", "stanko", "mislav", "ana"]  |  ["stanko", "ana", "mislav"] |  "mislav" |


#### 입출력 예 설명

예제 #1
"leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #2
"vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #3
"mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.


## 내 풀이

### 첫 번째 풀이

```js
function solution(participant, completion) {
    let p = participant.filter(x => !completion.includes(x));
    let answer = p.join();
    return answer;
}
```

#### 정확성  테스트
테스트 1 〉	통과 (0.05ms, 30.1MB)
테스트 2 〉	실패 (0.05ms, 30.1MB)
테스트 3 〉	통과 (0.30ms, 30.1MB)
테스트 4 〉	통과 (0.99ms, 30.2MB)
테스트 5 〉	실패 (1.03ms, 30.3MB)

#### 효율성  테스트
테스트 1 〉	실패 (시간 초과)
테스트 2 〉	실패 (시간 초과)
테스트 3 〉	실패 (시간 초과)
테스트 4 〉	실패 (시간 초과)
테스트 5 〉	실패 (시간 초과)

`filter()` 메서드로 participant에서 completion의 중복 데이터를 제거하여 완주하지 못한 사람을 배열로 만들고 `join()` 메서드로 문자열로 변환하여 answer로 반환하려 했으나, 이와 같은 방법을 사용하면 동명이인을 출력하지 못하는 문제가 있었다. 그리고 효율성이 완전히 박살나버렸다.. 결과적으로는 동명이인이지만 완주하지 못한 사람을 출력하려면 participant와 completion 배열을 같은 방식으로 정렬하여 인덱스별로 비교한 후, 일치하지 않는 인덱스를 출력하는 방향으로 구현해야 했다.

### 두 번째 풀이

```js
function solution(participant, completion) {
    var answer = '';
    participant.sort();
    completion.sort();
    for(var i = 0 ; i < participant.length; i++){
        if(participant[i] !== completion[i]){
            answer = participant[i];
            break;
        }
    }
    return answer;
}
```

#### 정확성  테스트
테스트 1 〉	통과 (0.05ms, 30.1MB)
테스트 2 〉	통과 (0.08ms, 30.1MB)
테스트 3 〉	통과 (0.34ms, 30.1MB)
테스트 4 〉	통과 (0.59ms, 30.2MB)
테스트 5 〉	통과 (0.63ms, 30.1MB)

#### 효율성  테스트
테스트 1 〉	통과 (46.78ms, 41.3MB)
테스트 2 〉	통과 (76.78ms, 47.7MB)
테스트 3 〉	통과 (96.70ms, 52.2MB)
테스트 4 〉	통과 (106.42ms, 55.1MB)
테스트 5 〉	통과 (110.64ms, 53.6MB)

어떻게 하면 각 배열을 같은 방법으로 정렬할 수 있을까 검색해보던 중에, `Array.prototype.sort()` 메서드를 찾을 수 있었다. 

> #### Array.prototype.sort() <br>
sort 메서드는 배열의 요소를 정렬한다. 원본 배열을 직접 변경하며 정렬된 배열을 반환한다. sort 메서드는 기본적으로 오름차순으로 요소를 정렬한다. 한글 문자열인 요소도 오름차순으로 정렬된다.

`sort()` 메서드로 participant와 completion 배열을 각각 오름차순으로 정렬한 후, for 문으로 각 배열의 인덱스를 서로 비교하였다. 결과값인 answer에는 `participant[i] !== completion[i]` 일 경우 `participant[i]` 값을 반환하도록 하였다. 이렇게 하면, 동명이인인 경우에도 일치하지 않는 인덱스가 하나 생기기 때문에 정상적으로 완주하지 못한 사람이 반환된다.


### 다른 사람 풀이

- `Map()`을 사용한 풀이

```js
const myMap = new Map();

    for (const participant of participants){
        if(!myMap.get(participant)){
            myMap.set(participant, 1);
        }else{
            myMap.set(participant, myMap.get(participant)+1);
        }
    }

    for(const completion of completions){
        if(myMap.get(completion)){
            myMap.set(completion, myMap.get(completion)-1);
        }
    }
    
    for(const participant of participants){
        if(myMap.get(participant) && myMap.get(participant) >=1 ){
            answer = participant;
        }
    }
```

- Object를 사용한 풀이

```js
function solution(participants, completions) {
    var answer = '';
    

    const obj = {};
    for(const participant of participants){
        if(!obj[participant]){
            obj[participant] = 1;
        }else{
            obj[participant] += 1;
        }
    }
    
    for(const completion of completions){
        if(obj[completion]){
            obj[completion] -=1;
        }
    }
    
    for(const participant of participants){
        if(obj[participant] >= 1){
            answer = participant;
        }
    }
  ```
  
key로 이름을, value로 참가 인원수를 저장한다. 그리고, 완주자 배열을 반복시켜 value를 1씩 감소시킨다. value가 1 이상이면 완주하지 못한 인원이므로 return 한다. (사실 Map과 Object는 아직 공부가 부족해서 코드를 봐도 잘 이해가 되지 않았다. 추가적으로 공부를 한 후에 다시 이해하도록 하자.)