## 샘플문제: 채점 프로그램 만들기
1. 지시문
학생들의 답안을 채점하는 프로그램을 작성하시오.
    - 모든 문제는 객관식으로 1 ~ 5 사이의 정답을 가짐
    - 10 문제에 대한 학생의 답을 매개변수 (arguments) 를 이용하여 입력
    - 문제의 정답은 배열 (arrays) 를 이용하여 초기화 (아래 문제 정답 참조)
    - 답안의 유효성을 체크 (답안의 개수: 10, 답의 범위: 1 ~ 5), 맞지 않으면 오류를 출력
      - 답안의 개수가 틀린 경우, “문제는 10 문제입니다. 현재 {n} 개의 답안을 제출했습니다. 10 개의 답안을 제출하시오.”
      - 답의 범위가 틀린 경우, “답의 범위는 1 ~ 5 입니다. 범위 외의 답안이 있습니다.”
    - 문제당 10점으로 채점하고, 학점은 아래 “학점” 을 참고
    - 정상 출력은 정답 수와 점수를 출력하고, 학점도 같이 출력

학점: A+(100-95), A(94-90), B+(89-85), B(84-80), C+(79-75), C(74-70), D+(69-65), D(64-60), F(59-0)
 

유효성 체크 1: 답안 수 == 10
유효성 체크 2: 1 <= 답 <= 5
문제 정답 예시: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5

2. 핵심 개념
#배열 #분기문 #반복문 #매개변수 #표준출력 #atoi #stdlib #break

3. 부가 설명
- char to int (atoi: stdlib.h): <https://ko.wikipedia.org/wiki/Stdlib.h>
- 표준입출력: <https://www.tutorialspoint.com/cprogramming/c_input_output.htm>
- break: <https://www.tutorialspoint.com/cprogramming/c_break_statement.htm>
- malloc: <http://www.cplusplus.com/reference/cstdlib/malloc/>
- strlen: <http://www.cplusplus.com/reference/cstring/strlen/?>

cf> 검색했을 떄 나오는 코딩도장 홈페이지의 내용 많이 참고함.
 


### 답안의 내용 깨끗이 정리
~~~C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

const int NUMBER_OF_ANSWERS = 10;
const int ANSWERS[NUMBER_OF_ANSWERS] = {1, 2, 3, 4, 5, 1, 2, 3, 4, 5};

const int NUMBER_OF_GRADES = 9;
const int SCORES[NUMBER_OF_ANSWERS] = {95, 90, 85, 80, 75, 70, 65, 60, 0};
const char *GRADES[NUMBER_OF_GRADES] = {"A+", "A", "B+", "B", "C+", "C", "D+", "D", "F"};

int* charArrayToIntArray(char *target[], const int length);
int getNumberOfCorrectAnswers(const int answers[], const int target[], int length);
int calculateScore(int numberOfCorrectAnswers);
char* calculateGrade(int totalScore, const int scores[], const char *grades[], int length);
void printAnswers(char *who, const int target[], int length);

int main(int argc, char *argv[])
{
    if (argc != (NUMBER_OF_ANSWERS +1)) {
        printf("문제는 10 문제입니다. 현재 %d개의 답안을 제출했습니다. 10개의 답안을 제출하시오.\n", argc -1);
        printf("ex) ./gs 4 4 3 5 2 5 1 2 4 3\n");
        return -1;
    }

    int *studentAnswers = charArrayToIntArray(argv, argc); //입력받은 문자열을 int열로 바꾸기.

    for (int i=0; i < NUMBER_OF_ANSWERS; i++) {
        if (studentAnswers[i] >= 0 && studentAnswers[i] <= 5) {
            continue;
        } else {
            printf("답의 범위는 1~5입니다. 범위 외의 답안이 있습니다. \n");
            return -1;
        }
    }



    printf("\t 학점평가 시작\n");

    int numberOfCorrectAnswers = getNumberOfCorrectAnswers(ANSWERS, studentAnswers, NUMBER_OF_ANSWERS);
    int totalScore = calculateScore(numberOfCorrectAnswers);
    char *grade = calculateGrade(totalScore, SCORES, GRADES, NUMBER_OF_GRADES);

    printAnswers("정답", ANSWERS, NUMBER_OF_ANSWERS);
    printAnswers("학생", studentAnswers, NUMBER_OF_ANSWERS);
    printf("정답 수: %d\n", numberOfCorrectAnswers);
    printf("점수: %d\n", totalScore);
    printf("학점: %s\n", grade);

    return 0;

}


int* charArrayToIntArray (char *target[], const int length)
{
    int *result = malloc(sizeof(int) * NUMBER_OF_ANSWERS);

    for (int i=0; i<NUMBER_OF_ANSWERS; i++) {
        result[i] = atoi(target[i+1]);
    }

    return result;
}

int getNumberOfCorrectAnswers(const int answers[], const int target[], int length)
{
    int numberOfCorrectAnswers = 0;

    for(int i=0; i < length; i++)
    {
        if (answers[i] == target[i])
        {
            numberOfCorrectAnswers++;
        }
    }

    return numberOfCorrectAnswers;
}

int calculateScore(int numberOfCorrectAnswers)
{
    return numberOfCorrectAnswers * 10;
}

char *calculateGrade(int totalScore, const int scores[], const char *grades[], int length)
{
    char *grade="grade";
    for (int i = 0; i < length; i++)
    {
        if (totalScore >= scores[i])
        {
            grade = malloc(sizeof(char) * strlen(grades[i]));
            strcpy(grade, grades[i]);
            break;
        }
    }

    return grade;
}


void printAnswers(char *who, const int target[], int length)
{
    printf("%s: ", who);

    for (int i = 0; i <length; i++)
    {
        printf("%d ", target[i]);
    }
    printf("\n");
}
~~~


### 내가 주어진 답안을 보고 다시 쳐보면서 공부한 내용
~~~C
#include <stdio.h>
#include <stdlib.h>
//stdlib.h는 C 언어의 표준 라이브러리로, 문자열 변환, 의사 난수 생성, 동적 메모리 관리 등의 함수들을 포함하고 있다.
//<- atoi 함수(ASCII string to integer. 문자열을 정수로 바꾸기)가 선언된 헤더 파일
#include <string.h>
//<- strcpy 함수(string copy. 문자열을 다른 곳으로 복사)

// 원래 정답 관련
const int NUMBER_OF_ANSWERS = 10;
const int ANSWERS[NUMBER_OF_ANSWERS] = {1, 2, 3, 4, 5, 1, 2, 3, 4, 5};

// 채점(점수, 학점) 관련
const int NUMBER_OF_GRADES = 9;
const int SCORES[NUMBER_OF_ANSWERS] = {95, 90, 85, 80, 75, 70, 65, 60, 0};
const char *GRADES[NUMBER_OF_GRADES] = {"A+", "A", "B+", "B", "C+", "C", "D+", "D", "F"};

int* charArrayToIntArray(char *target[], const int length);
int getNumberOfCorrectAnswers(const int answers[], const int target[], int length);
int calculateScore(int numberOfCorrectAnswers);
char* calculateGrade(int totalScore, const int scores[], const char *grades[], int length);
void printAnswers(char *who, const int target[], int length);

int main(int argc, char *argv[])
{
    if (argc != (NUMBER_OF_ANSWERS +1)) {
        printf("문제는 10 문제입니다. 현재 %d개의 답안을 제출했습니다. 10개의 답안을 제출하시오.\n", argc -1);
        printf("ex) ./gs 4 4 3 5 2 5 1 2 4 3\n");
        return -1;
    }

    int *studentAnswers = charArrayToIntArray(argv, argc); //입력받은 문자열을 int열로 바꾸기.

    for (int i=0; i < NUMBER_OF_ANSWERS; i++) {
        if (studentAnswers[i] >= 0 && studentAnswers[i] <= 5) {
            continue;
        } else {
            printf("답의 범위는 1~5입니다. 범위 외의 답안이 있습니다. \n");
            return -1;
        }
    }

    //printAnswer에 포함되어 있음.
    //for (int j=0; j < NUMBER_OF_ANSWERS; j++)
    //{
    //    printf("%d", studentAnswers[j]);
    //}


    printf("\t 학점평가 시작\n");

    int numberOfCorrectAnswers = getNumberOfCorrectAnswers(ANSWERS, studentAnswers, NUMBER_OF_ANSWERS);
    //printf("정답 수: %d\n", numberOfCorrectAnswers);

    int totalScore = calculateScore(numberOfCorrectAnswers);
    //printf("점수: %d\n", totalScore);

    char *grade = calculateGrade(totalScore, SCORES, GRADES, NUMBER_OF_GRADES);
    //printf("학점: %s\n", grade);

    printAnswers("정답", ANSWERS, NUMBER_OF_ANSWERS);
    printAnswers("학생", studentAnswers, NUMBER_OF_ANSWERS);
    printf("정답 수: %d\n", numberOfCorrectAnswers);
    printf("점수: %d\n", totalScore);
    printf("학점: %s\n", grade);

    return 0;

}


int* charArrayToIntArray (char *target[], const int length)
{
    int *result = malloc(sizeof(int) * NUMBER_OF_ANSWERS);
    //메모리를 사용하려면 malloc 함수로 사용할 메모리 공간을 확보해야 합니다(memory allocation)

    for (int i=0; i<NUMBER_OF_ANSWERS; i++) {
        result[i] = atoi(target[i+1]);
    }

    return result;
}

int getNumberOfCorrectAnswers(const int answers[], const int target[], int length)
{
    int numberOfCorrectAnswers = 0;

    for(int i=0; i < length; i++)
    {
        if (answers[i] == target[i])
        {
            numberOfCorrectAnswers++;
        }
    }

    return numberOfCorrectAnswers;
}

int calculateScore(int numberOfCorrectAnswers)
{
    return numberOfCorrectAnswers * 10;
}

char *calculateGrade(int totalScore, const int scores[], const char *grades[], int length)
{
    char *grade="grade"; //여기서 grade를 설정해두어야 하는 이유..??
    for (int i = 0; i < length; i++)
    {
        if (totalScore >= scores[i])
        {
            grade = malloc(sizeof(char) * strlen(grades[i]));
            strcpy(grade, grades[i]); //strcpy 함수는 string copy. 뒷 문자열을 다른 곳(앞)으로 복사. (string.h 헤더 파일에 선언됨)
            break;
        }
    }

    return grade;
}


void printAnswers(char *who, const int target[], int length)
{
    printf("%s: ", who);

    for (int i = 0; i <length; i++)
    {
        printf("%d ", target[i]);
    }
    printf("\n");
}
~~~

결과화면

![image](https://user-images.githubusercontent.com/68533679/88653812-83892180-d107-11ea-978e-60b94e48eefc.png)

