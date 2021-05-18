이미 이전에 CS50에서 다룬 내용을 언급하기는 하겠지만, 여기서 더욱 자세히 다룰 것.
(이 내용만 봐도 됨)

## 다뤄야 하는 내용
- 구조체
- pointer
- 동적할당
- 배열
- 함수


## 구조체 
<- 이미 CS50의 'last_4_practice.md'에서 다룸(구조체와 캡슐화)

## pointer
<- 이미 CS50의 '3_과제수행_중_조사.md', '5_lecture.md'에서 다룸. (여기서는 \*p가 가리키는 것과 &n이 가리키는 것의 의미 를 출력하며 알아봄)



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

