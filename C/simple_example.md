

## 기본 출력
~~~{c}
#include <stdio.h>

int main()
{
    printf("Hello, World !");
    return 0;
}
~~~
=> Hello, World !


## print에서의 문자 등 표현.

~~~{c}
#include <stdio.h>

int main() {
    int a = 20;

    printf("%d", a);
    
    return 0;
}
~~~
=> 20


## 'if ((선언문) > 수)'의 해석
선언문 자체는 무시하고 선언문에서 정의되는 변수 자체를 봄.
~~~{c}
#include <stdio.h>

int main(void)
{
    int num;
    if ((num=70) > 5)
    {
        printf("hello");
    }
    return 0;
}
~~~
=> hello

##
