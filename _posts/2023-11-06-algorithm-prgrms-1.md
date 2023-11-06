---
title:  "231106 - 요격시스템 " 

categories:
  - Programmers
tags:
  - [Programming, C++, CPP]

toc: true
toc_sticky: true

date: 2023-11-06
last_modified_at: 2023-11-06
---
<br>

# 프로그래머스 - 요격시스템

## 제한 사항
 - 1 ≤ targets의 길이 ≤ 500,000
 - targets의 각 행은 [s,e] 형태입니다.
 - 이는 한 폭격 미사일의 x 좌표 범위를 나타내며, 개구간 (s, e)에서 요격해야 합니다.
 - 0 ≤ s < e ≤ 100,000,000

## 사전 풀이
가장 쉽게 생각해볼 수 있는 것은 이중 for문을 통해 전체 개구간(s, e)에 가장 많이 중첩되는 구간 을 찾고 지우면 될듯하다. 하지만 targets의 길이가 500,000을 넘어 이와 같은 방법으로는 시간 초과를 예상할 수 있다.

그러므로 다른 방법을 생각해보아야 하는데 탐욕법 알고리즘 중 회의실 배정 문제와 비슷한 아이디어로 문제를 해결할 수 있을 듯 하였다.

이 문제를 푸는 방법으로는 개구간 (s, e) 중 가장 작은 $s_{min}$를 가지는 $Seg_{min}$부터 시작하여 중첩된 구간을 삭제하면 된다.

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