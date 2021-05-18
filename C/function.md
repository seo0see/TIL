### input.txt 파일의 내용을 라인 번호와 함께 output.txt파일에 출력
[출처: https://ehclub.co.kr/1141 ]

### feof 함수
[출처: https://ddoddofather.tistory.com/entry/C%EC%96%B8%EC%96%B4-%ED%8C%8C%EC%9D%BC%EC%9E%85%EC%B6%9C%EB%A0%A5-feof%ED%95%A8%EC%88%98%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90]
파일의 끝에 도달했는지 여부를 확인할 때 사용. 파일의 끝에 도달하게 되면 0이 아닌 값 반환.

### #define
[출처: https://docs.microsoft.com/ko-kr/cpp/preprocessor/hash-define-directive-c-cpp?view=msvc-160 ]
#Define 는 식별자 또는 매개 변수화 된 식별자와 토큰 문자열을 연결 하는 매크로 를 만듭니다. 매크로가 정의된 후 컴파일러는 소스 파일에서 발생하는 각 식별자를 토큰 문자열로 대체할 수 있습니다.

### fgets 함수
[출처: https://modoocode.com/38 ]
stream에서 문자열을 받는다.
~~~{c}
#include <stdio.h>  // C++ 의 경우 <cstdio>
char* fgets(char* str, int num, FILE* stream);
~~~
스트림에서 문자열을 받아서 (num - 1) 개의 문자를 입력 받을 때 까지나, 개행 문자나 파일 끝(End-of-File) 에 도달할 때 까지 입력 받아서 C 형식의 문자열(str)로 저장한다. 개행 문자는 fgets 로 하여금 입력을 끝나게 하지만 이 문자 역시 str 에 저장한다. NULL 문자는 자동적으로 마지막으로 입력받은 문자 뒤에 붙는다.
- str: 읽어들인 문자열을 저장할 char 배열을 가리키는 포인터
- num: 마지막 NULL 문자를 포함하여, 읽어들일 최대 문자수. (이 값이 10이면 컴퓨터는 최대 9문자를 입력받음)
- stream: 문자열을 읽어들일 스트림의 FILE 객체를 가리키는 포인터. 특히, 표준 입력(stdin) 에서 입력을 받으려면 여기에 stdin 을 써주면 된다. (예를 들어 fgets (str, 100, stdin); 과 같이)

### exit 함수
[출처: https://rabbit3000.tistory.com/649 ]
현재의 C프로그램 자체를 완전 종료하는 기능을 가짐. 주로, 에러가 났을 때 강제 종료시키기 위해 if문 속에서 사용됨. 이때, 모든 것을 잘 정리해 놓고 종료함("모든 열려진 파일"들을 자동으로 닫고, 출력 버퍼 속에 데이터가 있으면 그것을 쓰기 완료시킴.).
exit문은 보통 exit(0), exit(1) 이 두가지 형태로 많이 쓰는데 프로그램 수행이 정상적으로 종료 된다면 0을 비정상적이면 1 이나 0이 아닌 기타 정수를 씁니다
cf> break는 반복문이나 스위치문,return은 함수, exit는 프로그램 단위로 제어을 하는 것.
