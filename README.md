# going-on-a-trip
# c program
# The first line gives the number of cities N. N is 200 or less. The second line gives the number M of cities in the travel plan. M is less than or equal to 1000. The next N lines are given N integers. The j-th number in the i-th line means connection information between the i-th city and the j-th city. If it is 1, it is connected, if it is 0, it is not connected. If A and B are connected, then B and A are also connected. The last line gives the travel plan. Let's print YES on the first line if possible and NO if not possible! 첫 줄에 도시의 수 N이 주어진다. N은 200이하이다. 둘째 줄에 여행 계획에 속한 도시들의 수 M이 주어진다. M은 1000이하이다. 다음 N개의 줄에는 N개의 정수가 주어진다. i번째 줄의 j번째 수는 i번 도시와 j번 도시의 연결 정보를 의미한다. 1이면 연결된 것이고 0이면 연결이 되지 않은 것이다. A와 B가 연결되었으면 B와 A도 연결되어 있다. 마지막 줄에는 여행 계획이 주어진다. 첫 줄에 가능하면 YES 불가능하면 NO를 출력해보자!
# 문제를 쉽게 풀기 위해서 Union-Find 알고리즘에 대해 알아보자! union(a,b)의 의미 : find(a)(= a의 부모노드)와 find(b)(= b의 부모노드) 를 비교하여 만약 다르다면 b의 부모노드를 a의 부모노드와 같게 한다. find(a)의 의미 : 자신이 가리키고 있는 부모를 재귀적으로 찾아 실질적인 부모노드를 가져온 뒤 자신의 부모로 삼음
#include <stdio.h>
#include <stdlib.h>
int *parents;
int findParent(int x)
{
    if (parents[x] == x)
        return x;

    parents[x] = findParent(parents[x]);
    return parents[x];
}

void unionSets(int x, int y)
{
    int parentX = findParent(x);
    int parentY = findParent(y);

    if (parentX < parentY)
        parents[parentX] = parentY;
    else
        parents[parentY] = parentX;
}
int main()
{
    int i, j;

    int N;
    scanf("%d", &N); //도시의  수  
    // parents 초기화
    parents = malloc(sizeof(int) * N);
    for (i = 0; i < N; i++)
        parents[i] = i; 
    int M;
    scanf("%d", &M); //여행계획에 속한 도시수  
    int input;
    for (i = 0; i < N; i++)
    {
        for (j = 0; j < N; j++)
        {
            scanf("%d", &input); //여행하는 도시들끼리의 연결여부 (2차원배열) 

            if (input == 1)
                unionSets(i, j);
        }
    }
    int isPossible = 0; 
    scanf("%d", &input); //여행계획을 입력  
    int prevParent = findParent(input - 1);
    for (i = 1; i < M; i++)
    {
        scanf("%d", &input);

        if (findParent(input - 1) != prevParent)
        {
            isPossible = -1;
            break;
        }
    }
    // 출력
    if (isPossible == 0)
        printf("YES");
    else
        printf("NO");

    return 0;
}
#python
cities = int(input())
total= int(input())
parent={i:i for i in range(1,cities+1)}
def parent_find(x):
    if x==parent[x]:
        return x
    p=parent_find(parent[x])
    parent[x]=p
    return  parent[x]
def union(x,y):
    x=parent_find(x)
    y=parent_find(y)
    if x!=y:
        parent[y]=x
for y in range(1,cities+1):
    maps=list(map(int,input().split()))
    for x in range(1,len(maps)+1):
        if maps[x-1]==1:
            union(y,x)
tour=list(map(int,input().split()))
r=set([parent_find(i) for i in tour])
if len(r)==1:
    print("YES")
else:
    print("NO")
#input-->3 3  0 1 0  1 0 1  0 1 0  1 2 3
#result--> YES
