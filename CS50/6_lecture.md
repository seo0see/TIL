## malloc과 포인터 복습
malloc 함수를 이용하는 것과 포인터의 의미 제대로 정리.
포인터를 만드는 것, 메모리를 할당하는 것, 값을 만드는 것은 모두 다 다름.   

## 배열의 크기 조정하기
**이것으로 배열의 문제점(동적 메모리할당에 추가적 메모리 필요)을 알고, 다른 자료구조의 필요성 느끼기**   
일정한 크기의 배열이 주어졌을 때 그 크기를 키우는 방법: 새 배열 만들고, 복붙하고 내용 추가.
~~~c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *list = malloc(3*sizeof(int));
    if (list==NULL) { return 1; }   // 포인터가 잘 선언되었는지 확인
    list[0]=1; list[1] = 2; list[2] = 3;

    int *tmp = malloc(4*sizeof(int));
    if (tmp==NULL) { return 1; }
    for (int i=0; i<3; i++) { tmp[i] = list[i]; }
    tmp[3] = 4;

    free(list);

    list = tmp;

    for (int i=0; i<4; i++) { printf("%i\n", list[i]); }

    free(list);
}
~~~
### 같은 것을 realloc 함수를 이용해서 하기
~~~c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *list = malloc(3*sizeof(int));
    if (list==NULL) { return 1; }   // 포인터가 잘 선언되었는지 확인
    list[0]=1; list[1] = 2; list[2] = 3;

    // tmp 포인터에 메모리를 할당하고 list의 값 복사. 이전 list는 free됨.
    int *tmp = realloc(list, 4*sizeof(int));
    if (tmp==NULL) { return 1; }

    list = tmp;
    list[3] = 7;

    for (int i=0; i<4; i++) { printf("%i\n", list[i]); }

    free(list);
}
~~~

## Linked list 구현
~~~c
#include <stdio.h>
#include <stdlib.h>

// 연결 리스트의 기본 단위가 되는 node 구조체 정의
typedef struct node
{
    int number; // node에서 저장되는 값
    struct node *next; //다음 node 주소 가리키는 포인터
}
node;


int main(void)
{
    // list라는 이름의 node 포인터 정하기. linked list의 가장 첫번째 노드 가리킴
    // 이 포인터는 현재 아무 것도 가리키고 있지 않기에 NULL로 초기화
    node *list = NULL;

    // 새로운 node를 위해 메모리를 할당하고 포인터 *n으로 가리키기
    node *n = malloc(sizeof(node));
    if(n==NULL) { return 1; }
    // "n->number"는 n이 가리키는 node의 number필드로 "(*n).number"와 동일
    n->number = 1; // n의 number 필드에 1 저장
    n->next = NULL; // n 다음에 정의된 node가 없으므로 NULL로 초기화
    list = n; //첫번째 node를 정의했기에 list포인터를 n으로 변경

    // list에 다른 node를 더 연결하기 위해 n에 새로운 메모리 다시 할당
    n = malloc(sizeof(node));
    if (n==NULL) { return 1; }
    n->number = 2;
    n->next = NULL;
    list->next = n; //list의 다다음 node를 n 포인터로 지정

    // 또 새로운 node 연결
    n = malloc(sizeof(node));
    if (n==NULL) { return 1; }
    n->number=3;
    n->next=NULL;
    list->next->next = n; // list의 다다다음 node를 n 포인터로 지정

    // linked list의 값들 순서대로 출력하기
    for (node *tmp = list; tmp != NULL; tmp = tmp->next)
    { printf("%i\n", tmp->number); }

    // 메모리 해제를 위해 list에 연결된 node들을 처음부터 방문하면서 free하기
    while (list != NULL)
    {
        node *tmp = list->next; //첫노드의 다음 노드. // 앞의 tmp와 무관함.
        free(list);
        list = tmp; //없앤 것 다음 것이 첫노드로 됨.
    }
}
~~~

## 연결리스트의 활용: 트리
~~~C
#include <stdio.h>
#include <stdlib.h>

typedef enum {false, true} bool; // boolean 타입 정의

// 이진 검색 트리의 노드 구조체
typedef struct node
{
    int number;
    struct node *left; // 왼쪽 자식 노드
    struct node *right; // 오른쪽 자식 노드
} node;

// 이진 검색 함수
bool search(node *tree) // *tree는 이진 검색 트리를 가리키는 포인터
{
    if (tree==NULL) { return false; } // tree가 비어있는 경우
    else if (50 < tree->number) { return search(tree->left); }
    else if (50 > tree->number) { return search(tree->right); }
    else { return true; }
}

//그냥 내가.. 
// 10,20.25,30,35,40,50이 들어있는 tree 만들고 이진 검색 함수 활용
// 근데 오류가 엄청 많이 남.. 이 오류는 후에 본격적으로 공부하게 되면 해보기.
// ★★다음에는 node 추가하고, 균형 맞추는 것도 하는 tree 구현하는 것 해보기★★
int main (void)
{
    node *tree = NULL;

    node *n = malloc(sizeof(node));
    if (n == NULL) { return 1; }
    n->number = 30;
    n->left = NULL;
    n->right = NULL;
    tree = n;

    node *n = malloc(sizeof(node));
    if (n == NULL) { return 1; }
    n->number = 20;
    n->left = NULL;
    n->right = NULL;
    tree->left = n;


    node *n = malloc(sizeof(node));
    if (n == NULL) { return 1; }
    n->number = 40;
    n->left = NULL;
    n->right = NULL;
    tree->right = n;


    node *n = malloc(sizeof(node));
    if (n == NULL) { return 1; }
    n->number = 10;
    n->left = NULL;
    n->right = NULL;
    tree->left->left = n;


    node *n = malloc(sizeof(node));
    if (n == NULL) { return 1; }
    n->number = 25;
    n->left = NULL;
    n->right = NULL;
    tree->left->right = n;


    node *n = malloc(sizeof(node));
    if (n == NULL) { return 1; }
    n->number = 35;
    n->left = NULL;
    n->right = NULL;
    tree->right->left = n;

    node *n = malloc(sizeof(node));
    if (n == NULL) { return 1; }
    n->number = 50;
    n->left = NULL;
    n->right = NULL;
    tree->right->right = n;

    search(&tree);

    // 나중에 free도 시켜주어야 함.
}
~~~

## 이 다음의 해시테이블. 트라이, 스택, 큐, 딕셔너리는 코드화 하지 않음.






