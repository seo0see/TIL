
예제중 이것이 좀 좋아서...
이렇게 재귀함수를 쓰는 것 익혀두기.
~~c
#include <stdio.h>

int holl(int a);
int zzack(int a);

int main(void)
{
    int a;

    printf("자연수 한 개를 입력하세요: ");
    scanf("%d", &a);

    if (a%2 == 0)
    {
        printf("%d", zzack(a));
        printf("\n");
    }
    else
    {
        printf("%d", holl(a));
        printf("\n");
    }
}

int holl(int a)
{
    if (a==1) { return 1; }
    else { printf("%d ", a); return holl(a-2); }
}

int zzack(int a)
{
    if (a==2) { return 2; }
    else
    {
        printf("%d ", a);
        return zzack(a-2);
    }
}
~~~

## 배열과 문자열 프로그램 예시

### 6명의 몸무게를 입력받아 몸무게의 평균을 출력하는 프로그램
cf> 입력 수가 정해져 있으면 띄어쓰기로 구분하든 enter로 구분하든 상관 없음. 입력만 제대로 이루어지면 됨.
~~~c
#include <stdio.h>

int main()
{
    float weight[6];
    float sum = 0.0;
    float avg = 0.0;

    printf("정수 10개를 입력하세요: ");

    for (int a=0; a<6; a++)
    {
        scanf("%f", &weight[a]);
        sum += weight[a];
    }

    avg = sum / 6.0;
    printf("%.1f\n", avg);

    return 0;
}
~~~

입력: 23.2 39.6 66.4 50.0 45.6 48.0      
출력: 45.5       


## command line
~~~c
#include <stdio.h>

int main(int argc, char *argv[])
{
    printf("hello, %s\n", argv[1]);
}
~~~
결과화면      
![image](https://user-images.githubusercontent.com/68533679/91302956-39688e00-e7e2-11ea-9d6c-5727e09f7bf8.png)
