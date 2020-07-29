~~~C
#include <stdio.h>
#include <stdlib.h> // atoi 함수(문자열을 정수로 바꾸기)
#include <string.h> // strcpy 함수(string copy. 문자열을 다른 곳으로 복사)

//점수로 학점 구하기
int global;

const int NUMBER_OF_GRADES = 9;
const char *SCORES[NUMBER_OF_GRADES] = {"95", "90", "85", "80", "75", "70", "65", "60", "0"};
const char *GRADES[NUMBER_OF_GRADES] = {"A+", "A", "B+", "B", "C+", "C", "D+", "D", "F"};

void printArray(char *who, const char *target[], int length);
char *calculateGrade(int score, const char *scores[], const char *grades[], int length);
void askScore();

int main(void)
{
    //학점이랑 점수 출력하는 부분
    printf("학점 프로그램\n종료를 원하면 \"999\"를 입력\n");
    printf("[ 학점 테이블 ]\n");
    printArray("점수", SCORES, NUMBER_OF_GRADES);
    printArray("학점", GRADES, NUMBER_OF_GRADES);

    while (1)
    {
        askScore();

        if (global == 999)
        {
            printf("학점 프로그램을 종료합니다.\n");
            return -1;
            break;
        }
        printf("입력한 학점은: %d\n", global);


        char *grade = calculateGrade(global, SCORES, GRADES, NUMBER_OF_GRADES);
        printf("%s\n", grade);

    }

    return 0;
}


void printArray(char *who, const char *target[], int length)
{
    printf("%s : ", who);

    for (int i = 0; i <length; i++)
    {
        printf("%-2s\t", target[i]);
    }
    printf("\n");
}

char *calculateGrade(int score, const char *scores[], const char *grades[], int length)
{
    char *grade = NULL;
    for (int i = 0; i < length; i++)
    {
        if (score >= atoi(scores[i]))
        {
            grade = malloc(sizeof(char) * strlen(grades[i]));
            strcpy(grade, grades[i]);
            break;
        }
    }

    return grade;
}

void askScore()
{
    int input;
    printf("성적을 입력하세요 (0 ~ 100) : ");
    scanf("%d", &input);

    if ((input != 999) && (input <= 0 || input >= 100))
    {
        printf("성적을 올바르게 입력하세요! (0 ~ 100)\n");
        askScore();
    }
    else global = input;
}



~~~

결과화면

![image](https://user-images.githubusercontent.com/68533679/88818829-1dc99200-d1fa-11ea-8e21-260ede18357b.png)

