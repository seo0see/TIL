
### 포인터
~~~C
#include <stdio.h>

int main(void)
{
    int n = 50;
    int *p = &n;

    printf("%p\n", &n); //n의 주소
    printf("%i\n", *&n); // n의 값

    printf("%p\n", p);  //p는 n의 주소를 가리키는 포인터
    printf("%i\n", *p); //p의 위치에 위치한 값을 가져옴.
}
~~~
결과화면   
![image](https://user-images.githubusercontent.com/68533679/89892129-2b771280-dc11-11ea-86ed-cfc25330f2c5.png)   


### string 자료형 정의
~~~C
#include <stdio.h>

// string 자료형 정의
typedef char *string;

int main(void)
{
    string s = "EMMA";
    printf("%s\n", s);

    char *t = "DAVID";
    printf("%s\n", t);
}
~~~
결과화면   
![image](https://user-images.githubusercontent.com/68533679/89892737-164eb380-dc12-11ea-8a70-882d08d28758.png)   

### 문자열과 포인터
~~~c
#include <stdio.h>

int main(void)
{
    char *t = "DAVID";
    printf("%s\n", t);
    printf("%p\n", t);

    //t라는 배열(t는 그 배열의 첫번째 값의 주소를 나타냄)의 인덱스 별 주소
    printf("%p\t", &t[0]);
    printf("%p\t", &t[1]);
    printf("%p\t", &t[2]);
    printf("%p\n", &t[3]);

    //*t[index]로 할 경우, 아래의 오류가 남
    // indirection requires pointer operand ('int' invalid)

    // 그 주소의 값 불러오기
    // printf("%c\n", t);    이렇게 하면 오류가 남. t는 그냥 주소의 별칭.
    //우리는 주소를 가져올 것이 아니라, 그 주소가 가리키는 값을 가져와야 하므로 *t를 써야함.
    // printf("%s\n", *t);  이것도 안됨. %s의 정의 때문인듯.
    printf("%c\n", *t);
    printf("%c\t", *(t+0));
    printf("%c\t", *(t+1));
    printf("%c\t", *(t+2));
    printf("%c\t", *(t+3));
    printf("%c\n", *(t+4));
}
~~~
결과화면   
![image](https://user-images.githubusercontent.com/68533679/89894477-18664180-dc15-11ea-8153-6b2630149e7f.png)   

### (나 스스로) get_string 함수 구현하기
~~~C
#include <stdio.h>
#include <stdlib.h> //malloc함수

char *get_string(char *print);

int main(void)
{

    char *s = get_string("s: ");
    char *t = get_string("t: ");

    printf("%s, %s\n", s,t);
}

char *get_string(char *print)
{
    printf("%s", print);

    char *str = malloc(sizeof(char)*20);
    scanf("%s", str);

    printf("You entered: %s\n", str);
    return str;
    //여기서의 malloc은 free되는 것인가
}
~~~

결과화면   
![image](https://user-images.githubusercontent.com/68533679/89895722-4187d180-dc17-11ea-9bfc-323679ed0297.png)   

### 문자열 비교하기

* 이렇게 (s==t)는 s와 t의 주소를 비교하는 것. 
~~~C
#include <stdio.h>
#include <stdlib.h>

char *get_string(char *print);

int main(void)
{

    char *s = get_string("s: ");
    char *t = get_string("t: ");

    printf("%s, %s\n", s,t);

    if (s==t)
    {
        printf("Same\n");
    }
    else
    {
        printf("Different\n");
    }
}

char *get_string(char *print)
{
    printf("%s", print);

    char *str = malloc(sizeof(char)*20);
    scanf("%s", str);

    return str;
}
~~~
결과화면   
![image](https://user-images.githubusercontent.com/68533679/89896172-f28e6c00-dc17-11ea-93c5-15555f84f8df.png)   

* 값 비교하게끔 제대로!    
(*s == *t)는 그 문자열의 값을 비교하는 것이기는 하지만, 첫 값만 비교하는 것.
for loop를 사용하여 그 문자열의 값 하나하나 비교 가능.
~~~C
#include <stdio.h>
#include <stdlib.h> //malloc함수
#include <string.h> //strlen함수

char *get_string(char *print);

int main(void)
{

    char *s = get_string("s: ");
    char *t = get_string("t: ");

    printf("%s, %s\n", s,t);

    if (strlen(s) != strlen(t)) { printf("Different\n"); return 0;}

    for (int i=0, n=strlen(s)-1; i<n; i++)
    {
        if (*(s+i) != *(t+i)) { printf("Different\n"); return 0;}
    }

    printf("Same\n");
    return 0;
}


char *get_string(char *print)
{
    printf("%s", print);

    char *str = malloc(sizeof(char)*20);
    scanf("%s", str);

    return str;
}
~~~
결과화면   
![image](https://user-images.githubusercontent.com/68533679/89897400-ec00f400-dc19-11ea-9c67-b9262f1bbc40.png)   
### 메모리 할당과 해제
valgrind 라는 프로그램을 사용하면 우리가 작성한 코드에서 메모리와 관련된 문제가 있는지를 쉽게 확인 가능. 
~~~c
#include <stdlib.h>

void f(void)
{
    int *x = malloc(10 * sizeof(int));
    x[10] = 0;
}

int main(void)
{
    f();
    return 0;
}
~~~
![image](https://user-images.githubusercontent.com/68533679/89899585-7bf46d00-dc1d-11ea-95f4-494f524e1f64.png)

### 메모리 교환, 스택, 힙
***여기서 가장 크게 배운 것은, 나는 함수 밖의 값을 바꾸려면 전역변수를 써야 하는 줄 알았음.     
그런데 이렇게 포인터를 쓰면 쉽게 해결할 수 있는 문제였음***
~~~c
#include <stdio.h>

void swap(int *a, int *b);

int main(void)
{
    int x = 1;
    int y = 2;

    printf("x is %i, y is %i\n", x, y);
    swap(&x, &y);
    printf("x is %i, y is %i\n", x, y);
}

void swap(int *a, int *b)
{
    int tmp = *a;   //중요!
    *a = *b;    //중요!
    *b = tmp;   //중요!
}
~~~
결과: x is 1, y is 2      x is 2, y is 1

### 파일 쓰기
~~~c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    FILE *file = fopen("phonebook.csv", "a"); //a(덧붙이기),w(쓰기),r(읽기)이 있음
    char *name = get_string("Name: ");
    char *number = get_string("Number: ");
    fprintf(file, "%s, %s\n", name, number);
    fclose(file);
}
~~~




