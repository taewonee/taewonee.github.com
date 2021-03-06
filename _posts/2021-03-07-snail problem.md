달팽이는 1부터 N*N까지의 숫자가 시계방향으로 이루어져 있다.

다음과 같이 정수 N을 입력 받아 N크기의 달팽이를 출력하시오.

N이 3일 경우,
1 2 3
8 9 4
7 6 5

N이 4일 경우,
1 2 3 4
12 13 14 5
11 16 15 6
10 9 8 7


```
#include <iostream>
using namespace std;

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
