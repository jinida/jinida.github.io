---
title:  "231109 - 과제 진행하기" 

categories:
  - Programmers
tags:
  - [Programming, C++]

toc: true
toc_sticky: true

date: 2023-11-09
last_modified_at: 2023-11-09
---
<br>

# 프로그래머스 - 과제 진행하기

 - 문제 링크: https://school.programmers.co.kr/learn/courses/30/lessons/176962

## 제한 사항
 - 3 ≤ plans의 길이 ≤ 1,000
 - 진행중이던 과제가 끝나는 시각과 새로운 과제를 시작해야하는 시각이 같은 경우 진행중이던 과제는 끝난 것으로 판단합니다.

## 사전 풀이

직관적인 문제였던 것 같다. 과제 시작 시각을 기준으로 정렬한 후 문제 풀이 시간이 다음 시간을 초과할 경우 그 문제를 남겨두고 초과하지 못하는 경우 답에 추가되는 방식으로 문제가 해결되며 답의 경우 과제의 시작 시각이 아닌 최근 풀었던 과제 순으로 답이 저장된다.

## 풀이

```cpp
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

int getElapsedTimeForTwoTime(string firstTime, string secondTime)
{
    int firstMinute = stoi(firstTime.substr(3));
    int firstHour = stoi(firstTime.substr(0, 2));
    
    int secondMinute = stoi(secondTime.substr(3));
    int secondHour = stoi(secondTime.substr(0, 2));
    
    int retElapsedTime = (secondHour - firstHour) * 60 + secondMinute - firstMinute;
    return retElapsedTime;
}

vector<string> solution(vector<vector<string>> plans) {
    vector<string> answer;
    vector<pair<string, int>> remainPlans;
    sort(plans.begin(), plans.end(), [&](vector<string> plan1, vector<string> plan2)
    {
        return plan1[1] < plan2[1];
    });

    for (int i = 0; i != plans.size() - 1; ++i)
    {
        vector<string> curPlan = plans[i];
        vector<string> nextPlan = plans[i + 1];
        int remainTime = getElapsedTimeForTwoTime(curPlan[1], nextPlan[1]);
        int curWorkTime = stoi(curPlan[2]);
        
        if (remainTime == curWorkTime)
        {
            answer.push_back(curPlan[0]);
        }
        else if (remainTime < curWorkTime)
        {
            remainPlans.push_back(make_pair(curPlan[0], curWorkTime - remainTime));
        }
        else
        {
            answer.push_back(curPlan[0]);
            remainTime = remainTime - curWorkTime;
            
            while (remainTime != 0 && !remainPlans.empty())
            {
                auto remainPlan = remainPlans.back();
                remainPlans.pop_back();
                if (remainPlan.second <= remainTime)
                {
                    remainTime = remainTime - remainPlan.second;
                    answer.push_back(remainPlan.first);
                }
                else
                {
                    remainPlan.second = remainPlan.second - remainTime;
                    remainPlans.push_back(remainPlan);
                    remainTime = 0;
                }
            }
        }
    }

    answer.push_back(plans.back()[0]);
    while (!remainPlans.empty())
    {
        answer.push_back(remainPlans.back().first);
        remainPlans.pop_back();
    }
    return answer;
}
```