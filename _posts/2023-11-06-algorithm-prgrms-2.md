---
title:  "231107 - 두 원 사이의 정수 쌍 " 

categories:
  - Programmers
tags:
  - [Programming, C++]

toc: true
toc_sticky: true

date: 2023-11-07
last_modified_at: 2023-11-07
---
<br>

# 프로그래머스 - 두 원 사이의 정수 쌍

 - 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/181187

## 제한 사항
 - 1 ≤ r1 < r2 ≤ 1,000,000

## 사전 풀이
문제의 그림을 확인 하였을 때 각 반지름 $r_i$을 가지는 모든 원 안의 점들은 일정한 비율을 가지는 것처럼 보여 일반항을 구하여 풀이할 수 있지 않을까 생각하여 손으로 1시간 반정도 그려보았고 실패하였다.

그러므로 그 다음 방법인 각 높이 별로 안쪽 원과 바깥 쪽 원의 좌표 거리내에 정수 값을 구하는 것으로 문제를 해결할 수 있었다.

하지만 처음에 문제를 풀고 제출하였을 때는 통과가 되지않았는데 올림과 내림 연산에 대해 신경을 쓰지 않고 문제를 풀어 생기는 문제였다.
## 풀이

```cpp
#include <string>
#include <vector>
#include <math.h>
#include <iostream>
using namespace std;

long long solution(int inCircleR, int outCircleR)
{
    long long answer = outCircleR - inCircleR + 1;
    
    int distance;
    int inCircleX, outCircleX;
    long long inCircleYPow2, outCircleYPow2;
    long long outCircleRPow2 = pow(outCircleR, 2);
    long long inCircleRPow2 = pow(inCircleR, 2);
    for(int i = 1; i < outCircleR; ++i)
    {
        outCircleYPow2 = pow(i, 2);
        outCircleX = static_cast<int>(sqrt(outCircleRPow2 - outCircleYPow2));
        
        if (i < inCircleR)
        {
            inCircleYPow2 = pow(inCircleR - i, 2) ;
            inCircleX = ceil(sqrt(inCircleRPow2 - inCircleYPow2));
            answer += outCircleX - inCircleX + 1;
        }
        else
        {
            answer += outCircleX;
        }
    
    }
    answer *= 4;
    return answer;
}
```