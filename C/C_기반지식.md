이미 이전에 CS50에서 다룬 내용을 언급하기는 하겠지만, 여기서 더욱 자세히 다룰 것.
(이 내용만 봐도 됨)

## 다뤄야 하는 내용
- [구조체](#구조체(struct)) :star:  
- [pointer](#pointer) :star:  
- [동적할당](#동적-메모리-할당(dynamic-memory-allocation)) :star:  
- 배열  
- 함수  


## 구조체(struct)
<- 이미 CS50의 'last_4_practice.md'에서 다룸(구조체와 캡슐화)  
\[출처: https://blog.naver.com/htt12253/222121401483 , https://dojang.io/mod/page/view.php?id=408 ]  
- 서로 다른 자료형을 묶은 것. 다른 말로, 사용자 정의 자료형.  
- 배열과 어느 정도 똑같지만, 서로 다른 배열을 복사할 수 있음.  
- (배열이 같은 타입의 변수 집합이라고 한다면, 구조체는 다양한 타입의 변수 집합을 하나의 타입으로 나타낸 것)  
- 여러 개의 데이터를 따로따로 관리할 수 있게 만듦.& 각기 다른 여러 자료형들을 함수에 한 번에 넘겨줄 수 있음.  
- 기본 타입만으로는 나타낼 수 없는 복잡한 데이터 표현 가능.  
- 구조체는 보통 main 함수 바깥에 정의. 만약 함수 안에 구조체를 정의하면 해당 함수 안에서만 구조체 사용 가능.  
- 생성 방법: struct 안에 변수 많이 넣기  
~~~{c}
struct 구조체이름 {
  자료형 멤버이름;
 };
~~~
- 호출 방법  
struct 함수명 변수명;  
변수명. 안의 변수  
- 예제  
~~~{c}
#define _CRT_SECURE_NO_WARNINGS //strcpy 보안 경고로 인한 컴파일 에러 방지
#include <stdio.h>
#include <string.h> //strcpy 함수가 선언된 헤더 파일

struct Person { //구조체 정의
    char name[20];
    int age;
    char address[100];
};

int main()
{
    struct Person p1; //구조체 변수 선언

    // 점으로 구조체 멤버에 접근하여 값 할당
    strcpy(p1.name, "홍길동"); // 문자열을 다른 배열이나 포인터(메모리)로 복사 가능. strcpy(대상문자열, 원본문자열)
    p1.age = 30;
    strcpy(p1.address, "서울시 용산구 한남동");

    // 점으로 구조체 멤버에 접근하여 값 출력
    printf("이름: %s\n", p1.name);
    printf("나이: %d\n", p1.age);
    printf("주소: %s\n", p1.address);

    return 0;

}
~~~
- 구조체를 정의하는 동시에 변수를 선언할 수도 있음(닫는 중괄호와 세미콜론 사이에 변수를 지정).  
~~~{c}
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>

struct Person {
    char name[20];
    int age;
    char address[100];
} p1; // 구조체를 정의하는 동시에 변수 p1 선언 <- 이전의 것과 다른 부분

int main()
{
    //struct Person p1 ; // 없어짐.
    
    strcpy(p1.name, "홍길동");
    p1.age = 30;
    strcpy(p1.address, "서울시 용산구 한남동");

    printf("이름: %s\n", p1.name);
    printf("나이: %d\n", p1.age);
    printf("주소: %s\n", p1.address);

    return 0;

}
~~~
- 구조체 변수를 선언하는 동시에 초기화하기  
중괄호 안에 . (점)과 멤버 이름을 적고 값을 할당.  
struct 구조체이름 변수이름 = { .멤버이름1 = 값1, .멤버이름2 = 값2 };  
멤버 이름과 할당 연산자 없이 값만 콤마로 구분하여 나열해도 됨. 단, 구조체 멤버가 선언된 순서대로 , 각 멤버의 자료형에 맞게.  
struct 구조체이름 변수이름 = { 값1, 값2 };  
~~~{c}
#include <stdio.h>

struct Person {
    char name[20];
    int age;
    char address[100];
};

int main()
{
    // 구조체 변수를 선언하는 동시에 초기화 (1)
    struct Person p1 = {.name = "홍길동", .age = 30, .address = "서울시 용산구 한남동"};

    printf("이름: %s\n", p1.name);
    printf("나이: %d\n", p1.age);
    printf("주소: %s\n", p1.address);

    // 구조체 변수를 선언하는 동시에 초기화 (2)
    struct Person p2 = {"고길동", 40, "서울시 서초구 반포동"};
    
    printf("이름: %s\n", p2.name);
    printf("나이: %d\n", p2.age);
    printf("주소: %s\n", p2.address);

    return 0;
}
~~~
- **typedef 키워드**  
\[출처: http://tcpschool.com/c/c_struct_intro, https://dojang.io/mod/page/view.php?id=409 ]  
이미 존재하는 타입에 새로운 이름을 붙일 때 사용.  
구조체 변수를 선언하거나 사용할 때는 매번 struct 키워드를 사용하여 구조체임을 명시해야하지만,  
typedef 키워드를 사용하여 구조체에 새로운 이름을 선언하면 매번 struct 키워드를 사용하지 않아도 됨.  
~~~{c}
#define _CRT_SECURE_NO_WARNINGS // strcpy 보안 경고로 인한 컴파일 에러 방지
#include <stdio.h>
#include <string.h> //strcpy 함수가 선언된 헤더 파일

typedef struct _Person { //구조체 이름은 _Person
    char name[20];
    int age;
    char address[100];
} Person; //typedef를 사용하여 구조체 별칭을 Person으로 정의

int main()
{
    Person p1; //구조체 별칭으로 변수 선언
    
    // 점으로 구조체 멤버에 접근하여 값 할당
    strcpy(p1.name, "홍길동");
    p1.age = 30;
    strcpy(p1.address, "서울시 용산구 한남동");

    // 점으로 구조체 멤버에 접근하여 값 출력
    printf("이름: %s\n", p1.name);
    printf("나이: %d\n", p1.age);
    printf("주소: %s\n", p1.address);

    return 0;

}
~~~
cf> 구조체의 정의와 typedef 선언을 동시에 할 때에는 구조체의 이름을 생략할 수도 있음.  
~~~{c}
//typedef로 구조체의 별칭 만들기
typedef struct 구조체이름 {
  자료형 멤버이름;
} 구조체별칭;

// 변수 선언
구조체별칭 변수이름;
~~~
cf> 만약 구조체 별칭을 사용하지 않고 구조체 이름으로 변수를 선언하고 싶다면 struct 키워드에 구조체 이름으로 변수를 선언하면 됨. 아래에서 struct \_Person p1;과 Person p1;은 완전히 같음.  
~~~{c}
struct _Person p1;    // 구조체 이름으로 변수 선언
~~~
cf> typedef 활용하기  *<- 이 내용은 나중에 따로 이해 필요*
typedef는 자료형의 별칭을 만드는 기능이므로, 구조체뿐만 아니라 모든 자료형의 별칭을 만들 수 있습니다.
> typedef 자료형 별칭
> typedef 자료형* 별칭
다음은 int를 별칭 MYINT, int 포인터를 별칭 PMYINT로 정의하는 예제. 별칭으로 변수와 포인터 변수를 선언한다는 점만 다를 뿐 사용 방법은 일반 변수, 포인터와 같습니다.
~~~{c]
typedef int MYINT;      // int를 별칭 MYINT로 정의
typedef int* PMYINT;    // int 포인터를 별칭 PMYINT로 정의

MYINT num1;        // MYINT로 변수 선언
PMYINT numPtr1;    // PMYINT로 포인터 변수 선언

numPtr1 = &num1;   // 포인터에 변수의 주소 저장
                   // 사용 방법은 일반 변수, 포인터와 같음       
~~~
이처럼 typedef로 정의한 별칭을 사용자 정의 자료형, 사용자 정의 타입이라 부릅니다.

여기서 PMYINT는 안에 \*가 이미 포함되어 있으므로 포인터 변수를 선언할 때 \*를 붙여버리면 이중 포인터가 되므로 사용에 주의해야 합니다.
~~~{c}
PMYINT *numPtr1;    // PMYINT에는 *가 이미 포함되어 있어서 이중 포인터가 선언됨
int* *numPtr2;      // PMYINT *와 같은 의미. 이중 포인터
~~~

## pointer
\[출처: https://dojang.io/mod/page/view.php?id=275, https://m.blog.naver.com/PostView.naver?blogId=highkrs&logNo=220172673948&proxyReferer=https:%2F%2Fwww.google.com%2F ] *<-두번째 출처의 내용 자세히 읽어보는 것 좋을 듯.* 
<- 이미 CS50의 '3_과제수행_중_조사.md', '5_lecture.md'에서 다룸. (여기서는 \*p가 가리키는 것과 &n이 가리키는 것의 의미 를 출력하며 알아봄)
- **C 언어에서 메모리 주소는 pointer 변수에 저장.**
- 다음과 같이 포인터 변수는 \*를 사용하여 선언. 포인터 변수는 포인터로 줄어서 부르기도 함
> 자료형 \*포인터이름;
> 포인터 = &변수;
포인터를 선언할 때는 자료형을 알려주고 \*를 붙이는 방식을 사용함. 만약 변수가 int형이면 이 변수의 메모리 주소를 저장하는 포인터는 int\*이어야 함.

~~~{c}
#include <stdio.h>

int main()
{
    int *numPtr; //포인터 변수 선언
    int num1 = 15; // int형 변수를 선언하고 10 저장

    numPtr = &num1; //num1의 메모리 주소를 포인터 변수에 저장
    
    // 이 두 출력값 같게 나옴. 둘 다 메모리 주소로, 컴퓨터마다, 실행할 때마다 달라짐.
    printf("%p\n", numPtr); //포인터 변수 numPtr의 값 출력 => 포인터와 메모리 주소는 같은 의미
    printf("%p\n", &num1); //변수 num1의 메모리 주소 출력
    
    // 이 두 출력값도 같게 나옴. 둘다 15가 결과로 나옴.
    printf("%d\n", num1);
    printf("%d\n", *numPtr);

    return 0;
}
~~~
이 예시에서. numPtr은 10이 저장된 메모리 공간, 즉 변수 num1이 있는 공간을 가리키게 됨.   
![image](https://user-images.githubusercontent.com/68533679/118915140-6314a700-b967-11eb-8c43-dda021772d55.png)
추가적으로,  
![image](https://user-images.githubusercontent.com/68533679/118915208-80497580-b967-11eb-936b-5ec26ef7c770.png)
![image](https://user-images.githubusercontent.com/68533679/118915449-f3eb8280-b967-11eb-9acc-7fb2c179801a.png)

 cf> 추가로 구조체 포인터 사용하기(https://dojang.io/mod/page/view.php?id=418) 보는 것도 추천.



## 동적 메모리 할당(dynamic memory allocation)
<- 이미 CS50의 '3_과제수행_중_조사.md'(자세히 다룸), '5_lecture.md'(간단히 코드)에서 다룸.  
[출처: (주요) https://m.blog.naver.com/PostView.naver?blogId=jsky10503&logNo=221260798099&proxyReferer=https:%2F%2Fwww.google.com%2F , (추가) https://dojang.io/mod/page/view.php?id=285 ]  
컴퓨터 프로그래밍에서 실행 시간 동안 사용할 메모리 할당하는 것. (실행하는 순간 프로그램이 사용할 메모리의 크기를 고려하여 메모리의 할당이 이루어지는 정적 메모리할당과는 대조적)  
- 장점: 상황에 따라 원하는 크기만큼의 메모리가 할당되고 이미 할당된 메모리라도 언제든 크기 조정이 가능함 (실행 시간에 크기가 결정되는 동적 배열 및 리스트와 같은 경우는 힙을 사용하는 것이 보다 공간을 효율적으로 활용할 수 있음.)  
- 단점: 더 이상 사용하지 않을 때 명시적으로 메모리를 해제해 주어야 함. (C 언어의 경우, 사용한 공간을 명시적으로 반환해주어야 하므로 이 과정을 생략하게 되면 메모리 누수의 원인이 되기도 함.  )  
- 관련함수 (stdlib.h에 정의)  
> void *malloc(size_t size); : size 바이트의 메모리를 힙에서 할당하여 반환  
> void free (void *ptr): ptr이 가리키는 메모리 해제.  
  
~~~{c}
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int Value;
    
    // Value에 값 입력받아 저장.
    scanf("%d", &Value);

    // (int 크기 * 입력받은 크기)만큼 동적 메모리를 할당
    // 포인터 변수 numPtr에 int 크기에 입력받은 크기를 곱하여 메모리를 할당함.
    // numPtr에는 할당된 메모리의 주소를 담음.
    int *numPtr = malloc(sizeof(int) * Value);
    
    // 할당한 공간 하나하나에 0~(Value-1)를 담음.
    for (int i = 0; i < Value; i++)
    {
        numPtr[i] = i;
    }

    // 위의 array값 하나하나 출력
    for (int i = 0; i < Value; i++)
    {
        printf("%d\n", numPtr[i]);
    }

    free(numPtr); //동적 할당 메모리 해제

    return 0;
}
~~~
=>  
![image](https://user-images.githubusercontent.com/68533679/118639688-1fa92400-b813-11eb-80e0-b47f7d3c4141.png)

