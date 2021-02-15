---
title: "Welcome to Jekyll!"
date: 2017-10-20 08:26:28 -0400
categories: jekyll update
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

​```python
def print_hi(name):
  print("hello", name)
print_hi('Tom')
​```

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].


일단 기억나는대로 주절거리기

Programmars 디스크 컨트롤러 문제

각 작업에 대해 [작업이 요청되는 시점, 작업의 소요시간]을 담은 2차원 배열 jobs가 매개변수로 주어질 때,

작업의 요청부터 종료까지 걸린 시간의 평균을 가장 줄이는 방법으로

처리하면 평균이 얼마가 되는지 return 하도록 solution 함수를 작성해주세요.

(단, 소수점 이하의 수는 버립니다)

이 때 가장 시간을 줄이는 방법은 Greedy Algorithm인 가장 일찍 끝나는 일부터 처리하는 것이다.

작성 코드는 아래와 같다

시간이 오래걸렸던 이유를 다소 변명하자면, input이 시점 순으로 정렬되어있다고 처음에 가정했으나, 시점 순으로 정렬되어 있지 않아, PQ를 하나 더 사용해야했다.

공부가 된 점: Priority Queue를 적재적소에 사용하기.
Compare Struct를 다루기
 아래 처럼 정의하면, top()에 작은 수가 온다. 흠,,,, 그렇다고 한다. 
 less<int> 를 넣으면 큰놈부터 나온다. 점점 작아진다고 생각하면 편할듯
  
 ``` struct compare{
    bool operator()(vector<int> a, vector<int> b){
        return a[1] > b[1];
    }
};
```
 

```
//greedy 기법
//가장 빨리 끝나는 작업 순으로 진행
//input은 시점 순으로 정렬되어있다고 가정 -> 아니었다 
// 기다리지 말고 바로 연산해버리는 형태로?

#include <string>
#include <vector>
#include <queue>
using namespace std;

struct compare{
    bool operator()(vector<int> a, vector<int> b){
        return a[1] > b[1];
    }
};
struct cp{
    bool operator()(vector<int> a, vector<int> b){
        return a[0] > b[0];
    }
};

int solution(vector<vector<int>> jobs) {
    int answer = 0;
    int time = 0;
    int onworktill=0;
    int bug = 0;
    priority_queue<vector<int>,vector<vector<int>>,cp> jobpq;
    priority_queue<vector<int>,vector<vector<int>>,compare> works;
    for(int i =0; i<jobs.size(); i++)
        jobpq.push(jobs[i]);
    while(true) // till works are all done
    {
        if(jobpq.empty() && works.empty())
            break;
        while(! jobpq.empty() && time >= jobpq.top()[0])
        {
            works.push(jobpq.top());
            jobpq.pop();
        }
        if(! works.empty() && time>=onworktill){
            onworktill = time + works.top()[1];
            answer += onworktill - works.top()[0];
            works.pop();            
        }
        time ++;
    }
    return answer/jobs.size();
}
```


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
