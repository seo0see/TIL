## 숫자 애너그램 찾기

‘애너그램’이란 문자를 재배열하여 다른 뜻을 가진 단어로 바꾸는 것을 말합니다. 예를 들면 영어의 ‘tea’와 ‘eat’과 같이, 각 단어를 구성하는 알파벳의 구성은 같지만 뜻은 다른 두 단어를 말합니다. 우리말에는 ‘문전박대’와 ‘대박전문’과 같은 예를 들 수 있습니다. 우리는 문자 대신 숫자를 이용해서 애너그램을 찾는 프로그램을 만들어봅시다. 5자리의 숫자 1쌍이 입력으로 주어지며 애너그램일 경우에는 “True”를 아닐 경우에는 “False”를 출력하도록 합시다. 숫자를 입력받는 부분은 따로 구현하지 않고 프로그램 내부에 배열로 선언하는 것으로 가정하고, 숫자에는 중복이 있을 수 있습니다.

예)   
입력값: 12345, 54321 -> 출력값: True   
입력값: 14258, 25431 -> 출력값: False   
입력값: 11132, 21131 -> 출력값: True


**핵심개념**: 애너그램, 정렬알고리즘


### 문제풀이
~~~C
#include <stdio.h>

//두 입력값에 대해 모두 정렬하고 정렬한 그 값이 일치하는지 보기

int* sort(int array[]);

int main(void)
{
    printf("숫자 애너그램 찾기\n");
    int num1[5] = {1,1,1,3,2};
    int num2[5] = {2,1,1,3,1};

    int* sort1 = sort(num1);
    int* sort2 = sort(num2);

    char *flag = "True";
    for (int i=0; i<5; i++)
    {
        if (sort1[i] != sort2[i])
        { printf("%d %d \n",sort1[i], sort2[i]); flag = "False"; break; }
    }

    printf("%s\n", flag);
    return 0;

}

//버블 정렬로 정렬
int* sort(int array[])
{
    int temp;
    for (int i=0; i<5; i++)
    {
        for (int j=0; j<5-i-1; j++)
        {
            if (array[j]>array[j+1])
            {
                temp = array[j];
                array[j] = array[j+1];
                array[j+1] = temp;
            }
        }
    }
    for (int k=0; k<5; k++)
    { printf("%d", array[k]); }
    printf("\n");

    return array;
}
~~~

결과화면   
![image](https://user-images.githubusercontent.com/68533679/89366653-3c57ed80-d712-11ea-839b-d9c5ad0bb7bd.png)
