
예제중 이것이 좀 좋아서...
이렇게 재귀함수를 쓰는 것 익혀두기.
~~~c
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


## 구조체와 캡슐화
***Q typedef struct{}___;와 struct ___{}; 의 차이*** 
* 구조체에 학생정보(이름, 국어/수학/영어 점수, 평균)을 저장하고, 평균을 계산하여 출력하는 프로그램 작성하기
~~~c
#include <stdio.h>
#include <string.h>


typedef struct student
{
    char name[20];
    int korean, math, english;
    double ave;
} student;

int main()
{
    struct student s1;
    struct student s2 = {"김하하", 80, 94, 72};
    //Q. struct 배열을 가지고서 할 때, 꼭 모두 다 정의해야하나?
    //마지막의 것 안했을때, missing field 'ave' initializer 오류가 생김.

    s1.korean = 98;
    s1.math = 88;
    s1.english = 100;
    strcpy(s1.name, "박호호");

    printf("%d, %d, %d", s1.korean, s1.math, s1.english);

    s1.ave = (double)(s1.korean + s1.math + s1.english) / 3;
    s2.ave = (double)(s2.korean + s2.math + s2.english) / 3;

    printf("%s : %f\n", s1.name, s1.ave);
    printf("%s : %f\n", s2.name, s2.ave);
}
~~~
출력결과
박호호 : 95.333333 \n 김하하 : 82.00000

* 5명의 이름과 키를 입력받아 키가 가장 작은 사람의 이름과 키를 출력하는 프로그램 작성
~~~c
#include <stdio.h>
#include <string.h>

struct people
{
    char name[20];
    int height;
};

int main()
{
    struct people p[5]; //구조체 역시 배열 선언이 가능함.
    int min = 1000; // 가장 작은키
    int min_index = 5; // 그 키의 index

    for (int i=0; i<5; i++)
    {
        printf("5명의 '이름 키'를 입력하세요.\n")
        scanf("%s %d", p[i].name, &p[i].height);
        // &p[i]name이면 "format specifies type 'char *' but the argument has type 'char (*)[20]'"오류, p[i].height이면 "format specifies type 'int *' but the argument has type'int'"오류.
        // 원래는 &__로 그 변수를 가리키는 것의 주소룰 써야하는데, name자체가 그 배열의 주소를 가리키는 이름이니까 이것이 가능한듯.(확실하지 않음. 내 생각.)
    }

    // 최솟값과 그 인덱스 정하기.
    for (int i=0; i<5; i++)
    {
        if (min>p[i].height)
        {
            min = p[i].height;
            min_index = i;
        }
    }

    printf("%s : %d\n", p[min_index].name, p[min_index].height);

}
~~~


