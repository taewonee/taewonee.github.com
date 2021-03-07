아래 표와 같이 표시될 떄, 좌측 상단 부터 우측 하단까지 어떻게 가야 가장 적은 작업을 할 수 있을까
기본적으로 dijkstra라고 느꼈는데, bfs로도 풀 수 있을 것 같다

```
// BFS로도 되고 Dijkstra도 되는 것 같다
// priority queue struct를 한번 생각해보면 훨씬 쉽다
// pair로 구현하려고 했는데 val을 비교하려면 trio여야했다.( prioirity queue)
#include <iostream>
#include <queue>
using namespace std;
pair<int, int> direction[4] = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}}; //to check adjcent

class triple
{
public:
    int val, x, y;
    triple(int a, int b, int c) : val(a), x(b), y(c){};
    bool operator<(const triple a) const
    {
        return this->val > a.val;
    }
};

void update_adj(int **map, int **sol, bool **sel, triple temp, int N, priority_queue<triple> *pq)
{
    int x = temp.x;
    int y = temp.y;
    int val = temp.val;
    if (sel[x][y] == 1)
        return; //if it's the least, return
    for (int i = 0; i < 4; i++)
    {
        int x_new = x + direction[i].first;
        int y_new = y + direction[i].second;
        if (x_new < 0 || x_new >= N) //out of boundary
            continue;
        if (y_new < 0 || y_new >= N) //out of boundary
            continue;
        if (sol[x_new][y_new] > val + map[x_new][y_new]) //if smaller value is discovered
        {
            //            cout << "x: " << x << " y: " << y << " val: " << val << " x_new: " << x_new << "y_new: " << y_new << "new_val: " << val + map[x][y] << " map_val: " << map[x][y] << endl;
            sol[x_new][y_new] = val + map[x_new][y_new];
            pq->push(triple(sol[x_new][y_new], x_new, y_new));
        }
    }
}
int main()
{
    int N;
    cin >> N;
    int **map = new int *[N];   // supplied map
    int **sol = new int *[N];   // tracking the least value way
    bool **sel = new bool *[N]; // checking whether it is the least or not
    for (int i = 0; i < N; i++)
    {
        map[i] = new int[N];
        sol[i] = new int[N];
        sel[i] = new bool[N];
    }
    //array declare
    for (int i = 0; i < N; i++)
        for (int j = 0; j < N; j++)
            scanf("%1d", &map[i][j]);
    for (int i = 0; i < N; i++)
    {
        for (int j = 0; j < N; j++)
        {
            sel[i][j] = 0;
            sol[i][j] = 999;
        }
    }
    sol[0][0] = 0;
    priority_queue<triple> pq; //dist, x, y
    pq.push(triple(0, 0, 0));
    while (!pq.empty())
    {
        triple temp = pq.top();
        pq.pop();
        update_adj(map, sol, sel, temp, N, &pq);
        sel[temp.x][temp.y] = 1;
    }

    //print the solution
    cout << sol[N - 1][N - 1];
    cout << endl;
    for (int i = 0; i < N; i++)
    {
        for (int j = 0; j < N; j++)
            cout << sol[i][j] << " ";
        cout << endl;
    }

    //deallocate
    for (int i = 0; i < N; i++)
    {
        delete[] map[i];
        delete[] sol[i];
        delete[] sel[i];
    }
    delete[] map;
    delete[] sol;
    delete[] sel;
    return 0;
}
```
