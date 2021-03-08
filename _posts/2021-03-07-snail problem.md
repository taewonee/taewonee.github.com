---

layout: post

title: Snail Problem

---

달팽이는 1부터 N*N까지의 숫자가 시계방향으로 이루어져 있다.

다음과 같이 정수 N을 입력 받아 N크기의 달팽이를 출력하시오.
```
N이 3일 경우,
1 2 3
8 9 4
7 6 5
```
```
N이 4일 경우,
1 2 3 4
12 13 14 5
11 16 15 6
10 9 8 7
```
훨씬 더 좋은 풀이가 있을 것 같지만, 시간내에 풀기위하여, 정말 한칸씩 돌아가면 된다고 생각했다. running 시간이 10 예제에 대해 30초였기 때문에 성능이 중요해보이지 않았다.
그래서 그냥 rightmove, leftmove, downmove, upmove를 만들어 해결했다.
input size X input size 의 dynamic 2d array를 만드는데, n =1 인 패딩을 추가하여 array의 끝을 쉽게 찾게 해주었다.
for문과는 다를게 없을 것 같고, 좀 더 세련되게 Search로 풀 수 있을 것 같은데 아직 방법을 모르겠다. 추후에 업데이트 하도록 하겠다.
```
#include <iostream>
using namespace std;
```
아래와 같이 move를 하는데 어차피 padding이 되어 있어 array 값이 0이 아니면 바로 return 0를 해버린다.
```
int rightmove(int **array, int n, int &x, int &y, int num)
{
    if (array[x][y + 1] != 0) //reached end
        return 0;
    array[x][y++] = num;
    return 1;
}

int downmove(int **array, int n, int &x, int &y, int num)
{
    if (array[x + 1][y] != 0) //reached end
        return 0;
    array[x++][y] = num;
    return 1;
}

int leftmove(int **array, int n, int &x, int &y, int num)
{
    if (array[x][y - 1] != 0) //reached end
        return 0;
    array[x][y--] = num;
    return 1;
}

int upmove(int **array, int n, int &x, int &y, int num)
{
    if (array[x - 1][y] != 0) //reached end
        return 0;
    array[x--][y] = num;
    return 1;
}
```
main 문은 while문을 이용하여 짰다.
마지막 하나가 남으므로 그냥 그 x, y에 대하여 cnt를 넣어주면 된다.
```
int main()
{
    int input;
    cin >> input;
    int **answer = new int *[input + 2];
    for (int i = 0; i < input + 2; i++)
        answer[i] = new int[input + 2];
    for (int i = 0; i < input + 2; i++)
        for (int j = 0; j < input + 2; j++)
        {
            if (i == 0 || i == input + 1 || j == 0 || j == input + 1)
                answer[i][j] = 1;
            else
                answer[i][j] = 0;
        }
    int cnt = 1;
    int x = 1;
    int y = 1;
    while (cnt < input * input)
    {
        while (rightmove(answer, input, x, y, cnt))
            cnt++;
        while (downmove(answer, input, x, y, cnt))
            cnt++;
        while (leftmove(answer, input, x, y, cnt))
            cnt++;
        while (upmove(answer, input, x, y, cnt))
            cnt++;
    }
    answer[x][y] = cnt;
    //print
    for (int i = 1; i <= input; i++)
    {
        for (int j = 1; j <= input; j++)
            cout << answer[i][j] << " ";
        cout << endl;
    }
    //deallocation
    for (int i = 0; i < input + 2; i++)
        delete[] answer[i];
    delete[] answer;

    return 0;
}
```
