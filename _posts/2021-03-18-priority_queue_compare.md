---

layout: post

title: Priority Queue compare class

---

custom type(class) 에 대해서 priority queue의 compare를 어떻게 할 수 있을지 한 번 알아보았다.

pair<int, int>로 2d array의 좌표를 저장하고, 2d array 값을 참조하여 비교하려 했으나, struct compare의 overload를 할 떄 2d array에 접근할 수 없어서
어쩔 수 없이 triple 이라는 class를 만들게 되었다.
그래서 triple을 비교하고자 했다.


운래 prioirity queue는 비교 class가 마지막 인자로 들어가게 된다. 
<type, 저장 타입(?) 비교 클래스> 이 순서로 들어가는데, 어차피 저장 타입이야 vector<type> 형태로 나타내는 것이 대부분(나는 그랬다)이기 때문에 비교 연산자를 잘 만드는게 중요했다
  아래와 같은 두가지 방법이 있었는데, class와 struct의 차이 그리고 overload를 잘 알면 쉽게 이해할 수 있다.
  
  ```
      priority_queue<triple, vector<triple>, Compare> pq;
    priority_queue<trio, vector<trio>, less<trio>> pp;
```
trio와 triple을 비교해보도록 하자

```

class trio
{
public:
    int first;
    int second;
    int third;
    bool operator<(trio a) const
    {
        return this->first < a.first;
    }
    trio(int a, int b, int c) : first(a), second(b), third(c){};
};

```
위 처럼 class를 선언하고, 직접 overload를 해주면 된다. compare는 <만 overload해줘도 충분히 비교가 가능한 것으로 보인다.
stability를 생각해보면 된다. >만 해도 되는지는 추후에 생각해보겠다.
여기서 중요한 점은 operator를 overload할 때 항상 const로 overload해줘야 한다는 것이다.

```

class triple
{
public:
    int first;
    int second;
    int third;
    triple(int a, int b, int c) : first(a), second(b), third(c){};
};

struct Compare
{
    //    bool operator
    bool operator()(const triple a, const triple b)
    {
        return a.first > b.first;
    }
};
```
반면 overload를 struct로 따로해주는 경우는 좀 다르다. const가 굳이 필요없지만, 코드가 길어진다.
overload는 당연히 인자 2개를 받아야한다.
triple은 class로 정의했지만 당연히 struct로도 가능하다.
위와 같은 방법을 통해서 custom type의 pq를 관리할 수 있따.

