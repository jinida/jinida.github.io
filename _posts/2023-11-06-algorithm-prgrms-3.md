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

이 문제의 키포인트는 누적합을 구하는 것이다. 누적 합을 구함으로써 부분합의 계산을 $O(1)$ 시간에 가능하게하며 또한 $(i, j)$의 구간 합이 주어진 수 $k$를 넘어가게 될 때 i를 증가시켜 구간 합을 감소시키며 $k$와 일치하는 구간 합을 구함으로써 문제를 해결하게 된다.

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