## 미션2. N의 약수들  [하]
양수 A가 N의 진짜 약수가 되려면, N이 A의 배수이고, A가 1과 N이 아니어야 한다.     
**cf> 나는 문제풀이 과정에서 답 구하기 위해 find_answer의 함수를 만듦. 이 함수는 또 다른 파일을 가지고 실행하고 확인하며 문제를 해결.    
=> 필요한 기능별로 따로 프로그램을 만들고, 그 프로그램을 기반으로 함수를 만드는 것이 좋은 듯. 프로그램 해석에도 그렇고.**
~~~c
#include <stdio.h>
#include <stdlib.h>

int *divisor;

// 비교함수. 오름차순으로 정렬하게끔!
//(값이 같을때만 0이면 되고, 값이 크거나 작을때는 음수와 양수를 반환하면 됨.)
int compare(const void * a, const void * b)
{ return (*(int*)a - *(int*)b); }

int find_answer(int *divisor, int number);

int main()
{
    // 적을 약수의 개수
    int  NUMBER;
    printf("입력할 수의 갯수(N의 약수를 입력하고, 프로그램을 통해 N을 구할 것): ");
    scanf("%d", &NUMBER);

    divisor = malloc(sizeof(int) * NUMBER);

    printf("%d개의 수 입력 시작!\n", NUMBER);
    for(int i = 0; i < NUMBER; i++)
    {
        int tmp;
        scanf("%d", &tmp);
        divisor[i] = tmp;
    }

    // 퀵 정렬 함수 qsort(정렬할 배열, 요소 개수, 요소 크기, 비교함수)
    qsort(divisor, NUMBER, sizeof(int), compare);

    int answer = find_answer(divisor, NUMBER);
    printf("answer: %d\n", answer);

    return 0;
}

//리스트의 것으로 나누어지지 않으면 리스트의 수의 약수 구하고, 필요하면 그에대해 업데이트
int find_answer(int *array, int number)
{
    int answer = array[number-1];
    for (int j=0; j<number-1; j++)
    {
        int compare = array[j];
        if (answer%compare != 0)
        {
            //printf("---비교해볼 리스트의 수: %d\n", compare);
            for (int k=1; k<=compare; k++)
            {
                if (compare%k == 0) //약수이면
                {
                    //printf("-비교할 약수: %d\n", k);
                    if (answer%k != 0)
                    {
                        answer = answer*k;
                        printf("현재 answer: %d\n", answer);
                    }
                }
            }

        }

    }
    return answer;
}
~~~

결과화면 (답만)     
![image](https://user-images.githubusercontent.com/68533679/91279557-9a7f6a00-e7c0-11ea-8991-1ddc405ab7cc.png)

결과화면 (상세결과)     
![image](https://user-images.githubusercontent.com/68533679/91279488-82a7e600-e7c0-11ea-8c00-f9c5e9a720a7.png)



