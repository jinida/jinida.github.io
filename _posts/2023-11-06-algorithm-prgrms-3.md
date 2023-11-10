---
title:  "231108 - 연속된 부분 수열의 합" 

categories:
  - Programmers
tags:
  - [Programming, C++]

toc: true
toc_sticky: true

date: 2023-11-08
last_modified_at: 2023-11-08
---
<br>

# 프로그래머스 - 연속된 부분 수열의 합
 - 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/178870

## 제한 사항
 - 1 ≤ sequence의 원소 ≤ 1,000 (sequence는 비내림차순으로 정렬되어 있습니다.)
 - 5 ≤ k ≤ 1,000,000,000 (k는 항상 sequence의 부분 수열로 만들 수 있는 값입니다.)

## 사전 풀이
사전에 한번 풀어본 문제였다. 해당 문제를 사전에는 부분 수열의 합을 전부 골라 최소 값을 찾는 방식으로 풀이를 하였는데 통과가 되었고 비슷한 다른 문제를 풀었을 때 이보다 나은 풀이 방법이 생각나 문제를 다시 풀게되었다.

이 문제의 키포인트는 누적합을 구하는 것이다. 또한 이 누적합을 구한 뒤 $

순서는 다음과같다.
1. 목록에 남은 구간 중 가장 작은 $s_{min}$를 가지는 $Seg_{min}$를 선택한다.
2. 구간의 시작인 targetSrc가 이전에 저장된 tempDst보다 클 경우 answer를 늘리고 tempDst를 갱신한다. (앞의 구간과 겹치는 부분이 없기 때문.)
3. 구간의 시작인 targetSrc가 이전에 저장된 tempDst보다 작을 경우 구간에 포함되어 있으므로 answer를 늘리지 않는다. 또한 tempDst가 targetDst보다 작을 경우 갱신한다. (겹치는 구간이 발생함. 다음에 들어올 구간에 대해 $Seg_{i-1}$과는 겹쳐도 $Seg_{i}$와 겹침이 발생하지 않을 수 있음.)
 
또한 $Seg_{min}$을 선택하는 이유는 우리가 요격하고자 하는 점 $x$는 $Seg_{min}$의 $s_{min}$보다 항상 큰 값을 지니기 때문이다. 요격하고자 하는 점 $x$가 $s_{min}$보다 작을 경우 구간에 포함하지 않게되며 최적해에 포함되지 않는다.

## 풀이

```cpp
#include <vector>
#include <algorithm>
using namespace std;
// O(N * log(N))

int solution(vector<vector<int>> targets)
 {
    int answer = 0;
    sort(targets.begin(), targets.end());
    
    int tempDst = 0;
    for (auto& targetElem : targets)
    {
        int targetSrc = targetElem[0];
        int targetDst = targetElem[1];
        
        if (tempDst <= targetSrc)
        {    
            tempDst = targetDst;
            ++answer;
        }
        else
        {
            tempDst = std::min(targetDst, tempDst);
        }
    }
    return answer;
}
```