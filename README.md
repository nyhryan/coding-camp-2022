2291012 남윤혁  
이 글은 편하게 깃허브에서도 읽어볼수 있습니다.  
[깃허브 링크](https://github.com/nyhryan/coding-camp-2022)

# 코딩캠프 2023 첫날 (2023.01.09)
오후에 c 언어 2단계 문제 조금 풀다가 저녁먹고 끝.  
121 번 동적 계획법 알고리즘 문제(행렬 곱셈 문제) 풀다가 전혀 안되서 인터넷에서 찾아봤는데 그것도 이해 안되서 내일 다시 공부해야 할 것 같음.  
아래는 오늘 푼 문제들 (모두 C언어 2단계임)


# C언어 2단계 - 01 : 1부터 n까지의 합에서 짝수만 더하는 재귀함수
10까지의 짝수 합  
= 10 + *(8 까지의 합)*   
= 10 + (8 + *(6 까지의 합)*)  
= ... 의 식으로 진행
```cpp
#include <stdio.h>

int EvenSum(int n) {
    if (n % 2) {                // 홀수면 짝수로 만들기
        --n;
    }
    if (n <= 2) {
        return n;               // n이 2 이하면 그냥 n 리턴
    }
    return n + EvenSum(n - 2);  // n + (n - 2) + ...

}

int main() {
    int n;
    
    scanf("%d", &n);
    printf("Evensum: %d\n", EvenSum(n));

    return 0;
}
```
# 02 : 숫자 더하기 재귀함수
문제 : 
```
1               = 1
1 + (1 + 2)     = 4
1 + (1 + 2) + (1 + 2 + 3) = 10
...
10까지 출력하기
(정렬은 안했음)
```
소스 : 
```cpp
#include <stdio.h>

int main() {
    // s = 줄 당 총 합
    int p, s = 0;

    // i = 첫째줄 ~ 10번째 줄
    for (int i = 1; i < 11; ++i) {                  
        s = 0;
        // j = 괄호로 묶은게 1~10개
        for (int j = 1; j < i + 1; ++j) {           
            p = 0;
            if (j > 1) {
                // k = 괄호 내의 반복
                printf("(");
                for (int k = 1; k < j + 1; ++k) {   
                    p += k;
                    printf("%d", k);
                    if (k != j) {
                        printf(" + ");
                    }
                }
                s += p;
                printf(")");
                if (j != i) {
                    printf(" + ");
                }
            }
            // 맨 첫 줄 출력할때의 if
            // 좀 괴랄하긴 한데 되긴 된다
            else {
                for (int k = 1; k < j + 1; ++k) {
                    p += k;
                    printf("%d", k);
                    if (i != 1) {
                        printf(" + ");
                    }
                }
                s += p;
            }
        }
        printf(" = %d\n", s);
    }

    return 0;
}
```
# 03 : 배열의 합 계산
출력 : 
```
 5  7  4  3 | 19
 6  1  8  3 | 18
 3  2  7  6 | 18
------------+---
14 10 19 12 | 55

     5  7  4  3
->   6  1  8  3  } 의 배열을 만들어서 끝 행,열에 합을 출력하는 문제
     3  2  7  6
```
```cpp
#include <stdio.h>

int main() {
    // 합을 저장하기 위해 가로세로 여유자리 + 1
    int a[4][5] = {{5, 7, 4, 3}, {6, 1, 8, 3}, {3, 2, 7, 6}};       

    for (int i = 0; i < 4; ++i) {
        if (i == 3) {
            printf("------------+---\n");
        }
        for (int j = 0; j < 5; ++j) {
            // 합을 출력할 차례가 아니면 배열 값 출력 + 합 저장
            if (j < 4) {                    
                printf("%2d ", a[i][j]);
                if (i < 3) {
                    a[i][4] += a[i][j];
                    a[3][j] += a[i][j];
                }
                continue;
            }
            printf("| %d", a[i][j]);        // 합을 출력할 차례
            a[3][4] += a[i][j];             // 총합 저장할 자리에 저장
        }
        printf("\n");                       
    }

    return 0;
}
```
# 04 : 복리 계산
문제 : 백만원을 연 이율 6.4%로 3년간 복리로 예치했을때의 3년뒤 총 금액은?  
공식 : `(최초 예금액) * (1 + 이율) ^ 예치기간 = 총금액 (기간은 월 단위)`

```cpp
#include <stdio.h>
#include <math.h> 

double bank(double money, double dur) {
    double result = 0;
    double interest = 1 + 6.4 / 12;             // 월이율 = 연이율 / 12
    result = money * pow(interest, dur / 12);

    return result;
}

int main() {
    printf("%.0lf", bank(1000000.0, 3));
    return 0;
}
```
# 05 : 두 점을 지나는 직선의 기울기와 y절편을 구하라
문제 : 구조체, 포인터를 활용해 두 점을 지나는 직선의 기울기, y절편을 구하는 함수 `f()`를 만들어라.
```cpp
#include <stdio.h>

typedef struct _Point {
    int x;
    int y;
} Point;

void f(Point a, Point b, double* slope, double* intercept) {
    // 기울기 = y증가량 ÷ x증가량
    *slope = (b.y - a.y) / (b.x - a.x);  

    // y = mx + k에서 한 점과 기울기 m을 대입하면 y절편인 k를 구할 수 있다.                       
    *intercept = a.y - (*slope) * a.x;
    printf("slope : %lf\nintercept : %lf", *slope, *intercept);
}

int main() { 
    Point a, b;
    double slope, intercept;

    printf("a(x, y) : ");
    scanf("%d %d", &a.x, &a.y);

    printf("b(x, y) : ");
    scanf("%d %d", &b.x, &b.y);

    f(a, b, &slope, &intercept);
    return 0;
}
```
# 06 : 두 행렬의 합 함수
문제 : 두 행렬을 인수로 받아서 더하는 함수를 만들어라.  
출력 : 
```
a = 
1 2 3 
2 3 4 
3 4 5 
4 5 6 

b = 
1 1 1 
0 0 0 
3 3 3 
4 3 2 

a + b =
2 3 4 
2 3 4 
6 7 8 
8 8 8 
```
소스 : 
```cpp
// 06 : 두 행렬의 합을 계산하는 함수

#include <stdio.h>

#define ROW 4
#define COL 3

void addArray(int a[][COL], int b[][COL], int result[][COL]) {
    for (int i = 0; i < ROW; ++i) {
        for (int j = 0; j < COL; ++j) {
            result[i][j] = a[i][j] + b[i][j];
        }
    }
}

void printArr(int arr[][COL]) {
    for (int i = 0; i < ROW; ++i) {
        for (int j = 0; j < COL; ++j) {
            printf("%d ", arr[i][j]);
        }
        printf("\n");
    }
}


int main() {
    int a[ROW][COL] = {{1, 2, 3}, {2, 3, 4}, {3, 4, 5}, {4, 5, 6}};

    int b[ROW][COL] = {{1, 1, 1}, {0, 0, 0}, {3, 3, 3}, {4, 3, 2}};

    int result[ROW][COL] = {
        0,
    };

    printf("a = \n");
    printArr(a);
    printf("\n");

    printf("b = \n");
    printArr(b);
    printf("\n");

    addArray(a, b, result);

    printf("a + b =\n");
    printArr(result);

    return 0;
}
```
# 07 : 최고 성적을 받은 학생은?
문졔 : 07: 학생들의 이름과 영어,수학 성적으로 구성된 구조체를 만들고 그 중 최고 성적을 받은 학생을 출력  
출력 : 
```
English Highscore : John (100)
Math Highscore : Park (90)
```
```cpp
#include <stdio.h>
#include <string.h>

typedef struct _Student {
        char name[10];
        int engScore;
        int mathScore;
} Student;

// 학생들의 정보 입력
void setData(int eng, int math, char* name, Student* s) {
    s->engScore = eng;
    s->mathScore = math;
    strcpy(s->name, name);
}

void compareScore(Student* arr) {
    int engHigh, mathHigh;
    engHigh = arr[0].engScore;
    mathHigh = arr[0].mathScore;

    char engName[10] = {0,};
    char mathName[10] = {0,};

    for (int i = 0; i < 3; ++i) {
        // 이전의 최고 점수보다 다음 배열값이 높으면 걔가 최고 점수
        // 그러면 그 점수를 받은 학생의 이름을 기억해놓음
        if (engHigh <= arr[i].engScore) {
            engHigh = arr[i].engScore;
            strcpy(engName, arr[i].name);
        }
        if (mathHigh <= arr[i].mathScore) {
            mathHigh = arr[i].mathScore;
            strcpy(mathName, arr[i].name);
        }
    }
    printf("English Highscore : %s (%d)\n", engName, engHigh);
    printf("Math Highscore : %s (%d)\n", mathName, mathHigh);
    
}

int main() {
    Student studentArr[3] = {0,};

    setData(100, 50, "John", &studentArr[0]);
    setData(70, 90, "Park", &studentArr[1]);
    setData(90, 80, "Kim", &studentArr[2]);

    compareScore(studentArr);
    return 0;
}
```
# 08 : 행열의 두 행을 서로 교체하는 함수
문제 : 교체하고 싶은 행렬의 두 행을 인수로 받아 교체해주는 함수를 만들어라.  
예시) 1, 2헹을 교체하고 싶다.
```
원본
1 1 1 
2 2 2 
3 3 3 

교체 후
2 2 2 ✅
1 1 1 ✅
3 3 3 
```
```cpp
// 08: 행렬의 두 행을 교환하는 함수를 작성
// ex) 1, 2를 입력받으면 1행과 2행을 교환함

#include <stdio.h>
#define ROW 3
#define COL 3

void swap(int a[][COL], int b[][COL], int x, int y) {
    if (x < COL && y < COL) {
        for (int i = 0; i < ROW; ++i) {
            for (int j = 0; j < COL; ++j) {
                if (i == x - 1) {               // x 행은 y행에 복사
                    b[y - 1][j] = a[i][j];
                }
                if (i == y - 1) {               // y행은 x행에 복사
                    b[x - 1][j] = a[i][j];
                } else {                        // 아니면 그냥 그대로 복사
                    b[i][j] = a[i][j];
                }
            }
        }
    }
    else {
        printf("Wrong x, y value!\n");
    }
}

void printArr(int* arr) {
    for (int i = 0; i < ROW * COL; ++i) {
        printf("%d ", *(arr + i));
        if (i % COL == 2) {
            printf("\n");
        }
    }
}

int main() {
    int a[ROW][COL] = {{1, 1, 1}, {2, 2, 2}, {3, 3, 3}};

    int b[ROW][COL] = {
        0,
    };

    printArr(a);
    printf("\n");

    swap(a, b, 1, 2);       // a 배열의 1, 2행을 서로 교체해서 b 배열에 복사
    printArr(b);

    return 0;
}
```
# 09 : 전치행렬 구하기 (transpose)
1행은 1열이 되고, 2행은 2열이 되는 그런 행렬인가보다.  
문제 : A행렬의 전치행렬을 B행렬에 복사해주는 프로그램을 만들어라.  
\- 전치행렬의 예)  
```
원본
11
22
33
44

원본의 전치행렬
1 2 3 4 
1 2 3 4  
```
```cpp
// 09: 전치행렬을 구하는 함수
// 1행은 1열로, 2행은 2열로...

#include <stdio.h>
#define ROW 4
#define COL 2

// a[0,1,2][0] = b[0][0,1,2]
// i, j만 바꿔주면 됨
void transpose(int a[][COL], int b[][ROW]) {
    for (int i = 0; i < COL; ++i) {
        for (int j = 0; j < ROW; ++j) {
            b[i][j] = a[j][i];  // a의 1,2,3열 = b의 1,2,3행
        }
    }
}
int main() {
    int a[ROW][COL] = {{1, 1}, {2, 2}, {3, 3}, {4, 4}};

    int b[COL][ROW] = {
        0,
    };

    transpose(a, b);    // a행렬의 전치행렬을 b에다가 복사

    for (int i = 0; i < COL; ++i) {
        for (int j = 0; j < ROW; ++j) {
            printf("%d ", b[i][j]);
        }
        if (i % COL == COL - 2) {
            printf("\n");
        }
    }

    return 0;
}
```
# 10 : 20~30살 사이의 직원 찾기
문제 : 직원 구조체를 만들고 직원 정보들 중에서 20~30살에 해당하는 직원의 정보를 출력해라.  
출력 : 
```
Name : Kim (20)
-> id: 123, tel; 010-1111-2222

Name : Park (25)
-> id: 444, tel; 010-3333-4444

Name : Hong (30)
-> id: 555, tel; 010-6666-1234
```
```cpp
// 10:  직원 구조체는 사번, 이름, 전화번호, 나이로 구성
// 5명의 데이터에서 나이가 20~30살인 사람을 찾아서 출력

#include <stdio.h>
#include <string.h>

#define MAX 5

typedef struct _Employee {
    int id;
    int age;
    char name[10];
    char tel[14];
} Employee;

// 직원 정보 등록
void setData(int id, int age, char* name, char* tel, Employee* arr) {
    arr->id = id;
    arr->age = age;
    strcpy(arr->name, name);
    strcpy(arr->tel, tel);
}

// 직원 정보 출력
void printData(Employee* e) {
    printf("Name : %s (%d)\n", e->name, e->age);
    printf("-> id: %d, tel; %s\n\n", e->id, e->tel);
}

// 20~30살의 직원 찾기
void find(Employee e[MAX]) {
    for (int i = 0; i < MAX; ++i) {
        if (e[i].age >= 20 && e[i].age <= 30) {     // 20살 ~ 30살이면 직원 정보 출력
            printData(&e[i]);
        }
    }
}

int main() { 
    Employee empArr[MAX] = {0,};

    // 배열 요소로 넘길 때는 참조연산자 붙이기
    // 단 [0] 요소는 그냥 넘기는거랑 똑같음
    // &empArr[0] -> empArr
    setData(123, 20, "Kim", "010-1111-2222", &empArr[0]);
    setData(444, 25, "Park", "010-3333-4444", &empArr[1]);
    setData(555, 30, "Hong", "010-6666-1234", &empArr[2]);
    setData(777, 31, "Nam", "010-8765-1234", &empArr[3]);
    setData(999, 19, "Lee", "010-6543-2345", &empArr[4]);

    // 배열 통으로 넘기면 없어도 됨
    find(empArr);

    return 0;
}
```
# 11 : 10진수를 8, 16진수로, 재귀함수로 출력하기
예시 : 
```
octal(123)  
= octal(123 / 8 = 15)  
= octal(15 / 8 = 1)  
= 0 출력, 리턴
= 1 출력 (1 % 8), 리턴
= 7 출력 (15 % 8), 리턴 
= 3 출력 (123 % 8), 리턴

✅ 최종 값 0173
```
소스 : 
```cpp
// 11: 1만 보다 작은 정수를 입력받아 8, 16진수로 변환하는 함수를
// 재귀함수로 만들고, 8진수는 0, 16진수는 0x를 앞에 붙여 출력

#include <stdio.h>

// 8진수
void octal(int n) {
    if (n < 1) {
        printf("0");
        return;
    } else {
        octal(n / 8);
        printf("%d", n % 8);
        return;
    }
}

// 16진수
// a = 97, b = 98, ...
void hexa(int n) {
    if (n < 1) {
        printf("0x");
        return;
    } else {
        hexa(n / 16);
        if (n % 16 > 9) {
            printf("%c", 87 + n % 16);
        } else {
            printf("%d", n % 16);
        }
        return;
    }
}

int main() {
    octal(123);
    printf("\n");

    hexa(123);

    return 0;
}
```
# 12 : 구구단 출력하기
이쁜 구구단 표
```
 *  1  2  3  4  5  6  7  8  9 
 1  1  2  3  4  5  6  7  8  9 
 2  2  4  6  8 10 12 14 16 18 
 3  3  6  9 12 15 18 21 24 27 
 4  4  8 12 16 20 24 28 32 36 
 5  5 10 15 20 25 30 35 40 45 
 6  6 12 18 24 30 36 42 48 54 
 7  7 14 21 28 35 42 49 56 63 
 8  8 16 24 32 40 48 56 64 72 
 9  9 18 27 36 45 54 63 72 81 
```
소스 : 
```cpp
#include <stdio.h>

int main() {
    for (int i = 1; i < 10; ++i) {
        // 맨 첫줄
        if (i == 1) {
            printf(" * ");
            for (int j = 1; j < 10; ++j) {
                printf("%2d ", j);
            }
            printf("\n");
        }
        // i가 2이상
        // 맨 첫 열에는 1, 2, 3, .. 출력함
        printf("%2d ", i);

        for (int j = 1; j < 10; ++j) {
            printf("%2d ", i * j);
        }
        printf("\n");
    }
    return 0;
}
```
# 코딩캠프 둘째 날
하루종일 행렬 연속 곱셈 알고리즘 문제를 풀었다. 인터넷 봐도 설명을 너무 괴랄하게 적어놔서 이해하는데 한참 걸렸다. 내 언어로 다시 한 번 정리해봤다. 내일은 그냥 앞 문제를 풀어야겠다...

# 121: Matrix Chain Multiplication (행렬 연속 곱셈)
행렬을 여러개 곱할 때, 어떤 행렬끼리 묶어서 (결합법칙) 곱하냐에 따라 곱하는 횟수가 다르다. 공대생답게 **몇개의 행렬을 곱할때 몇번 곱하는게 최소**인지 한번 찾아보는 문제이다.

\- 문제에서 입력받는 값 : 
```
mCount : 곱하려는 행렬의 개수
dim[mCount + 1] : 각 행렬들의 크기
```
> 참고:   
> 행렬은 앞 행렬-열 크기 / 뒷 행렬-행 크기가 같아야 곱할 수 있다.  
> 
> ![dim[]의 의미](https://imgur.com/A403Wz6.jpg)
> dim[mCount + 1] 배열은 각 행렬들의 크기를 담고 있다. 즉 아래처럼 된다.
> ```
> A 행렬 : dim[0] x dim[1]
> B 행렬 : dim[1] x dim[2]
> C 행렬 : dim[2] x dim[3]
> ```

곱하고 싶은 행렬의 개수가 아주 많다면 일일히 한개씩 묶어서 해보는건 좀 그렇다.  
그래서 우리는 **동적 계획법**을 통해 풀어볼 것이다. 

## 마법의 배열

일단 원리는 모르지만 이 문제의 **해답을 담고있는 배열**이 있다고 하자.  
> 이렇게 접근하는게 동적 계획법이라고 들은거같음  

A, B, C, D, E 다섯개의 배열이 있고, 우리는 이 5개를 모두 곱했을 때의 최소 곱셈 횟수를 알고 싶은 것이다.  

`m[i][j]` 라는 이 마법의 배열에는 i번째 ~ j번째 배열을 곱했을 때 그 최소 곱셈 횟수가 담겨 있다. 우리는 A (1번째) 부터 E (5번째) 까지 모두 곱했을 때의 그 해답이 알고 싶은 것이므로 m[1][5]를 보고 답을 알아내면 된다.  
> 배열은 0번째 부터 시작하는건 생각하지 말고 직관적으로 "1번째부터 5번째까지 곱했을 때의 해답" 이라고 생각하자.

## 공식 (점화식?)

![](https://imgur.com/ghjUMxd.jpeg)
예를 들어 ABCD 네개의 행렬을 곱한다고 하자.  
그렇다면 그림처럼 총 3가지의 경우가 있을테고, 그 중 최솟값을 구하면 그게 바로 `m[1][4]`일 것이다.  
`k`는 어디서 묶기 시작할 것인지 알려준다.

![](https://imgur.com/fkLfeZY.jpeg)
공식화 시키면 위와 같다. `m[i][j] = 0 (i == j)` 인 이유는 1번째 배열에서 1번째 배열 까지 곱하는 횟수는 0번이기 때문인 것과 같다. 왜 바로 이런 공식이 나왔는지 잘 모르겠으니 아래에 예시대로 차례차례 해보며 규칙을 찾아보자.

## 일단 해보기
![](https://imgur.com/xWZbkSE.jpg)
이 문제는 bottom-up 방식으로 푼다. 작은 문제로 위로 올라가면서 큰 문제를 푸는 뭐 그런거 같았던듯.  
1,2,3,4,5 단계를 살펴보면 k값의 범위가 바로 행렬을 어디서 묶을 건지에 대한 범위인 것을 알 수 있다.  
예를 들면 `ABCDE` 다섯개를 곱할 때면 묶을 수 있는 경우의 수가
```
A (BCDE)   | k = 1
(AB) (CDE) | k = 2
(ABC) (DE) | k = 3
(ABCD) E   | k = 4
-> ∴ 1 ≤ k < 5
1 = i 이고 5 = j 임을 알 수 있다.

(ABCDE, 행렬이 5개 이고 이는 곳 1번째에서 5번째까지 곱하는 최소 곱셈 횟수 m[1][5]를 구하는 과정...)
```
![](https://imgur.com/ieVigfR.jpeg)

우리가 알고 싶은 `m[1][5]`를 알기 위해서는 `m[1][4]` 혹은 `m[2][5]`가 필요하다.  
이런 식으로 계속 내려가다 보면 `m[1][2], m[2][3]...`을 구해놓으면 결국에는 m[1][5]를 푸는데 사용할 수 있다. (Dynamic Programming에서 memoization이라고 했던 방법같다.)
> 여기서 맨 처음, 즉 m[1][1], m[2][2]...를 포함해서 구했는데 지금 보니 어짜피 0이니깐 넘기고 계산해도 되었었다...  

### `m[1][2]`의 계산 예시
![](https://imgur.com/4q4wnQU.jpeg)
위와 같은식으로 상향식으로 진행해 나가면 원하는 답을 구할 수 있다.

## 의사코드 (Pseudo) / C언어 코드
![](https://imgur.com/lt0Yp6N.jpg)

우리가 문제를 푼대로 배열에 접근하기 위해서 처음에 배열을 1만큼 더 크게 잡는다.

```cpp
#include <limits.h>
#include <stdio.h>
#include <stdlib.h>

// prints n * n size array
void arrayPrint(int** arr, int size) {
    for (size_t i = 0; i < size; ++i) {
        if (i == 1) {
            for (size_t j = 0; j < size; ++j) {
                printf("-----");
            }
            printf("\n");
        }
        for (size_t j = 0; j < size; ++j) {
            if (j == 1) {
                printf("|");
            }
            printf("%4d ", arr[i][j]);
        }
        printf("\n");
    }
    printf("\n");
}

// free n * n array
void arrayFree(int** arr, int size) {
    for (size_t i = 0; i < size; ++i) {
        free(arr[i]);
    }
    free(arr);
}

// adds 0 1 2 3 ... on first row&column
void arrayPretty(int** arr, int size) {
    for (size_t i = 0; i < size; ++i) {
        arr[0][i] = (int) i;
        arr[i][0] = (int) i;
    }
}

int main() {
    int mCount;
    printf("How many matrices? : ");
    scanf("%d", &mCount);

    int* dim = (int*)malloc((mCount + 1) * sizeof(int));

    printf("Enter %d dimensions : ", mCount + 1);
    for (size_t i = 0; i < mCount + 1; ++i) {
        scanf("%d", dim + i);
    }

    int** m = (int**)malloc((mCount + 1) * sizeof(int*));
    int** k = (int**)malloc((mCount + 1) * sizeof(int*));

    for (size_t i = 0; i < mCount + 1; ++i) {
        m[i] = (int*)calloc((mCount + 1), sizeof(int));
        k[i] = (int*)calloc((mCount + 1), sizeof(int));
    }

    arrayPretty(m, mCount + 1);
    arrayPretty(k, mCount + 1);

    // 여기부터 문제푸는 곳
    for (size_t d = 1; d < mCount; ++d) {
        for (size_t i = 1; i <= mCount - d; ++i) {

            // 최소값을 일단 엄청 큰 수로 잡아둔다.
            int min = INT_MAX;
            int minK = 0;

            int j = (int) d + (int) i;
            
            // 어디서 묶냐(k 값 변화)에 따른 그 중 최소 곱셈 회수 찾기
            for (size_t k = i; k < j; ++k) {
                int calc = m[i][k] + m[k + 1][j] + dim[i - 1] * dim[k] * dim[j];

                // 계산한 곱셈 횟수가 이전 k값으로 계산한거보다 작으면
                // 얘가 새로운 최솟값
                // 이러면 MIN 매크로 함수를 안 만들어도 된다
                if (calc < min) {
                    min = calc;
                    minK = k;
                }
            }

            // k값을 다 돌려봐서 그 중에서 나온 최솟값을
            // 마법의 배열에 저장 (그 최솟값의 k값도 따로 저장함)
            m[i][j] = min;
            k[i][j] = minK;
        }
    }

    // 정답 출력
    printf("List of m[i][j] : \n");
    arrayPrint(m, mCount + 1);

    printf("Listt of k[i][j] : \n");
    arrayPrint(k, mCount + 1);

    // 메모리 해제
    arrayFree(m, mCount + 1);
    arrayFree(k, mCount + 1);

    return 0;
}
```
출력 : 
```
How many matrices? : 5
Enter 6 dimensions : 10 4 8 12 7 5

List of m[i][j] : 
   0 |   1    2    3    4    5 
------------------------------
   1 |   0  320  864 1000 1060 
   2 |   0    0  384  720  860 
   3 |   0    0    0  672  900 
   4 |   0    0    0    0  420 
   5 |   0    0    0    0    0 

Listt of k[i][j] : 
   0 |   1    2    3    4    5 
------------------------------
   1 |   0    1    1    1    1 
   2 |   0    0    2    3    4 
   3 |   0    0    0    3    3 
   4 |   0    0    0    0    4 
   5 |   0    0    0    0    0 
```
10x4, 4x8, ..., 7x5짜리 배열 5개가 있을때 이 모두를 곱할 때 최소 횟수는 `m[1][5] = 1060`이다.  
묶어야 하는 방법을 보기 위해서는 아래의 `k[][]`배열을 참고하면 된다.  
```
k[1][5] = 1
   1 2345                   // 1번째에서 묶기
-> A(BCDE)
      |
    2345
    BCDE에서 k[2][5] = 4
           234  5           // 4번째에서 묶기
    -> A ((BCD) E)
           |
        234
        BCD에서 k[2][4] = 3
                23  4       // 3번째에서 묶기
        -> A (((BC) D) E)

-> 따라서 A(((BC)D)E)) 순서대로 함.
```
# 동적할당과 2차원배열과 메모리 주소...
❗️**틀린** **정보를** **포함하고** **있을** **수도** **있습니다**❗️

동적할당된 2차원배열을 []를 안쓰고 그리고 반복문을 하나만 돌려서 출력하는 함수를 만들다가 막혀서 질문을 많이하다가 알게된 것을 정리한다.

![](https://imgur.com/N9RHWPH.jpg)

고정크기로 할당하면 3x3 배열의 9개의 요소가 연속된 주소값을 갖는다.
```
arr1[0][0] : 100
arr1[0][1] : 104
>> arr1[0][2] : 108    // ✅
----
>> arr1[1][0] : 112    // ✅ 1행에서 2행 넘어갈때도 +4 만큼 주소가 커진다.
arr1[1][1] : 116
...
arr1[2][2] : ...
```

근데 동적할당으로 배열을 선언하니까...
```
arr2[0][0] : 100
arr2[0][1] : 104
>> arr2[0][2] : 108    // ✅
----
>> arr2[1][0] : 116    // ?? 1행에서 2행 넘어갈때 8만큼 주소가 커진다.
arr2[1][1] : 120       // 🤯 다시 4씩 증가
...
arr2[2][2] : ...
```
행에서 행이 넘어갈 때 주소값 증가량이 항상 8인지는 확실하지는 않은데 계속 돌려보면 그렇게 나온다(?)  
아직 내가 메모리에 빠삭하지는 않아서 그런지 아무튼  
**동적할당**을 통해 배열을 선언할 때는 배열 요소 주소값이 **항상 연속적인 것은 아니다!** ~~근데 계속 행 넘어갈때만 8씩 늘고 평소에는 4씩 늘어간다 왜지~~

의문이 든건 고정크기 배열의 9개의 연속된 주소값이라면 `**(arr1 + i)`를 통해 i를 1씩 증가하면서 아홉번 출력하면 9개 요소를 전부 출력할 수 있지 않나 인데...
> 2차원배열은 이중포인터니까 arr1[0][0]부터 한칸씩 올라가자...하는 생각에서 비롯된 것 같다...  

돌려보면 **처음 세번은 각 행의 첫번째 요소만 출력되고 4번째 이후로는 쓰레기값이 나온다.**  
> 해결법은 반복문을 두번 돌려서
> ```cpp
> for (int i = 0; i < 3; ++i) {
>     for (int j = 0; j < 3; ++j) {
>         printf("%d ", *(*(arr1 + i) + j));
>                       // --------   --
>                       //i 번째 행   j번째 요소
>     }
>     printf("\n");
> }
> ```
> 의 형식으로 돌리면 된다. ` *(*(arr1 + i) + j)`라는 표현이 많이 괴랄하므로 그냥 `arr1[i][j]`를 이용하도록 하자(?)  
## 고정크기 배열과 동적할당 배열
### 1. 고정크기 배열
![](https://imgur.com/CLS3hQi.jpg)
위에서 말했듯이, 고정크기 배열은 원소개수만큼 통째로 메모리를 할당받는다. 덕분에, 반복문 하나만 돌려도 배열을 출력할 수 있다.  
방법 : 
```cpp
int arr1[3][3] = {{1,2,3}, {4,5,6}, {7,8,9}};

for (int i = 0; i < 9; ++i) {
    printf("%d ", *(*arr1 + i));
    if (i % 3 == 2) {
        printf("\n");
    }
}
// --------------------------------
// 위의 for문을 만약 함수로 한다면

void printArray(int (*arr)[3]) {
    for (int i = 0; i < 9; ++i) {
        printf("%d ", *(*arr + i));
        // 보기 안좋으니까
        // for문 두번 써서 아래처럼 하자...
        // printf("%d ", arr[i][j]); 
        if (i % 3 == 2) {
            printf("\n");
        }
    }
}
// --------------------------------
// ... main() 안에서 호출
printArray(arr1);
```
출력 : 
```
1 2 3
4 5 6
7 8 9
```
> `* 연산자`의 우선순위 때문에 우선은,  
> 1. `*arr1` = arr1[0]의 주소 = arr1[0][0]의 주소
> 2. `*arr1 + i` = arr1[0][0]의 주소부터 부터 한칸씩 올라가는 주소 (총 9번 이동)
> 3. `*(*arr1 + i)` = 2번의 주소에 들어있는 값    
> 
> 라고 생각되어 진다..

이런 방식으로 진행되어 반복문 하나로 배열을 출력할 수 있다.  
~~더럽게 복잡하니깐 그냥 편하게 for문 두번 쓰자.~~
### 참고 : 2차원배열을 함수인수로 넘기려면...
> 이런식으로 매개변수를 `array는 여러개의 COLUMN개짜리 배열을 묶어둔 것의 포인터`로 넘기던가 ~~뭔 소리야~~  
> 즉 `여러개의 COLUMN개의 1차원배열을 묶어둔 것중의 첫번째 배열의 주소 = 2차원배열의 맨 처음 요소의 주소`을 뜻하는 듯?
> ```cpp
> TYPE f(int (*array)[COLUMN]){...};
> ```
> 아래 방식으로 적는다. **위의 방법이 정석**이고,  
> 아래의 방법은 표기만 배열같은 것이지, 위의 표기와 같은 의미이다. 어짜피 배열을 넣어서 넘긴다 하더라도 컴파일러가 보면 배열은 곧 배열의 첫번째 요소를 가르키는 포인터와 같아서 이나저나 같다.
> ```cpp
> // ROW는 써도되고 안써도 됨
> TYPE f(int array[ROW][COLUMN]){...};
> ```
> 호출시에는 그냥 배열 이름만 넘기면 뚝딱
> ```cpp
> f(array1);
> ```
> 아니면 그냥 포인터로 매개변수를 만들어서 배열을 포인터로 캐스팅해서 넘겨도 되기는 한다.
> ```cpp
> void f(int* arr) {
>     for (int i = 0; i < 9; ++i) {
>         printf("%d ", *(arr + i));
>     }
> }
> // 호출 시...
> f((int *)array1);
> ```
> 이 방법은 1차원 배열을 매개변수로 쓸 때와 같다.  
> 1차원 배열을 매개변수로 쓸때는 아래와 같다. 셋 모두 같은 표현이고, 함수를 호출할때는 배열이름만 넣어서 호출해도 된다.
> ```cpp
> TYPE f(int arr[]){...};
> TYPE f(int arr[SIZE]){...};
> TYPE f(int* arr){...};
> ```
### 2. 동적할당 배열
![](https://imgur.com/mPVumRf.jpg)
동적할당으로 배열을 만들면 이중포인터를 사용하게된다.  
처음 여기서는 세개의 행을 가르키는 포인터의 묶음이다. (노랑 형광펜 부분)

```cpp
int** arr2 = (int **)malloc(3 * sizeof(int*));
```
> 즉 `arr2[0]`은 `100, 108, 116번지의 묶음`을 가르킨다. 즉 `arr2[0] (= arr2):  100번지`이다.   
> 그니깐 `int** arr2` 는 100번지를 가르키는 이중 포인터이다. (100번지는 200번지를 가르키고 있으므로 포인터의 포인터, 이중이다.)  
> 그래서 `3개 x 정수포인터의 크기`로 할당받아 `int **`로 캐스팅하는것

그 다음은 각 행이 가르키고 있는 3개의 요소(3열짜리)를 가진 묶음을 가르킨다. (파란 형광펜 부분)  
```cpp
for (int i = 0; i < 3; ++i) {
    arr2[i] = (int *)malloc(3 * sizeof(int));
}
```
> `100번지`는 `200, 204, 208번지의 묶음`을 가르킨다.  
> 그니깐 `arr2[0] = 100번지`는 `200번지`를 가르키는 그냥 포인터이다.  
> 그래서 `3개 x 정수크기`로 할당받아 `int *`로 캐스팅하는 것

동적할당을 사용하기 때문에, 배열의 각 행은 연속된 주소값을 가지지 않을 수도 있다.  
고정크기로 배열을 만들면 9개를 통째로 묶어서 주소를 주지만, 여기서는 3개씩 끊어서 만들다가, 그 사이에 어떤 일이 일어나면(?) 1행3열 다음 2행1열의 주소는 규칙적으로 이어서 주지않고 다른 곳에 할당해 준다.  
> 하지만 컴퓨터의 마법으로 1,2,3행이 서로 다른곳에 할당을 받아도 하나의 배열로 선언되어 있어서 연결리스트처럼 이어져 있는 거같다...? 

이때의 배열 출력방법
```cpp
int** arr2 = (int **)malloc(3 * sizeof(int*));
for (int i = 0; i < 3; ++i) {
    arr2[i] = (int *)malloc(3 * sizeof(int));
}
/// 배열 요소를 1,2,3...,7,8,9로 초기화했다고 치자

for (int i = 0; i < 3; ++i) {
    for (int j = 0; j < 3; ++j) {
        printf("%d ", *(*(arr + i) + j));
        // 복잡하니깐 이걸 쓰자
        // printf("%d", arr[i][j]);
    }
    printf("\n");
}
// 얘도 함수로 만든다면
void printArray2(int** arr) {
    for (int i = 0; i < 3; ++i) {
        for (int j = 0; j < 3; ++j) {
            printf("%d ", *(*(arr + i) + j));
            // 역시 복잡해보이니까
            // printf("%d ", arr[i][j]);
        }
        printf("\n");
    }
}
```
출력 : 
```
1 2 3
4 5 6
7 8 9
```
> `*(*(arr + i) + j)`의 의미 (위에 그림 참조)
> 1. `*(arr + i)` : i번째 행의 주소 (100, 108, 116번지)
> 2. `*(arr + i) + j` : i번째 행의 j번째 열의 주소 (200, 204, 208 / 250, 254, 258 번지)
> 3. `*(*(arr + i) + j)` : 위 2번 주소 안의 값

정도 되시겠다. ~~그니깐 제발 편하게 arr[i][j] 쓰자~~

# 코딩캠프 마지막 날
오늘은 앞으로 다시 가서 도전해 볼 만한 문제들 풀다가 연결리스트로 마무리했다. (역시 C언어 2단계)

# 55 : 동적할당을 이용하여, 단어 목록
문제 : 입력 받을 단어의 개수를 먼저 받고, 그만큼 동적할당한다. 그리고 40문자 이내의 단어들을 입력받아, 단어들을 출력하고 그리고 그중에서 가장 짧은 단어와 긴 단어를 출력한다.

출력 : 
```
How many words? : 5
> 1) Enter 5 words : love
> 2) Enter 5 words : Apple
> 3) Enter 5 words : peach
> 4) Enter 5 words : Hotdog
> 5) Enter 5 words : Pizzaaaa 

<Your Input>
[0] : love
[1] : Apple
[2] : peach
[3] : Hotdog
[4] : Pizzaaaa

> Longest Word : Pizzaaaa
> Shortest Word : love
```

소스 : 
```cpp
// 55 : 입력 받을 단어의 개수를 먼저 받고, 그만큼 동적할당한다
// 그리고 40문자 이내의 단어들을 입력받아, 단어들을 출력하고
// 그리고 그중에서 가장 짧은 단어와 긴 단어를 출력

#include <stdio.h>
#include <stdlib.h>
#include <strings.h>
#define MAX 40

int main() {
    int wordCnt;
    int lenMax, lenMin;
    int idxMax, idxMin;
    printf("How many words? : ");
    scanf("%d", &wordCnt);

    char** word = (char**)malloc(wordCnt * sizeof(char*));
    for (size_t i = 0; i < wordCnt; ++i) {
        word[i] = (char*)calloc(MAX, sizeof(char));
    }

    for (size_t i = 0; i < wordCnt; ++i) {
        char inputWord[MAX];
        printf("> %d) Enter %d words : ", i + 1, wordCnt);
        scanf("%s", inputWord);
        strcpy(word[i], inputWord);
        // printf("\n");
    }

    lenMax = strlen(word[0]);
    lenMin = strlen(word[0]);

    printf("\n<Your Input>\n");

    int len;
    for (size_t i = 0; i < wordCnt; ++i) {
        len = strlen(word[i]);
        if (len >= lenMax) {
            lenMax = len;
            idxMax = i;
        }
        if (len <= lenMin) {
            lenMin = len;
            idxMin = i;
        }
        printf("[%d] : %s\n", i, word[i]);
    }
    printf("\n> Longest Word : %s\n", word[idxMax]);
    printf("> Shortest Word : %s\n", word[idxMin]);

    return 0;
}
```
# 63 : 두 배열의 병합 후 정렬
문제 : 두 배열 a[5], b[5]를 병합하고 정렬하여 c[10]에 담는 프로그램 만들기  
출력 : 
```
a[5] : 1 2 3 7 8 
b[5] : 4 5 6 7 10 
> c[10] : 1 2 3 4 5 6 7 7 8 10   
```
소스 : 
```cpp
//  63 : 정렬이 되어 있는 두개의 배열 a[5], b[5]를 받아서
// 병합+정렬한 배열 c를 출력하는 프로그램

#include <stdio.h>

void merge(int a[], int b[]) {
    int aCnt = 0, bCnt = 0;
    int* pa = a;
    int* pb = b;
    int c[10];

    for (size_t i = 0; i < 10; ++i) {
        // a,b 중 한 쪽을 다 담기 전까지는
        // 둘 중 작은 값부터 차례로 c에 담음
        if (aCnt != 5 && bCnt != 5) {
            if (*pa < *pb) {
                c[i] = *pa;
                ++aCnt;
                ++pa;
            } else if (*pb <= *pa) {
                c[i] = *pb;
                ++bCnt;
                ++pb;
            }
        }
        // a, b 중 한쪽을 다 c에 담아서 끝난 경우
        // 나머지 배열의 나머지 요소들은 c에 차례로 담음
        else {
            if (aCnt == 5) {
                c[i] = *pb;
                ++pb;
            } else {
                c[i] = *pa;
                ++pa;
            }
        }
    }

    printf("> c[10] : ");
    for (size_t i = 0; i < 10; ++i) {
        printf("%d ", c[i]);
    }
}

int main() {
    int a[5] = {1,2,3,7,8};
    int b[5] = {4,5,6,7,10};

    printf("a[5] : ");
    for (int i = 0; i < 5; ++i) {
        printf("%d ", a[i]);
    }
    printf("\nb[5] : ");
    for (int i = 0; i < 5; ++i) {
        printf("%d ", b[i]);
    }
    printf("\n");

    merge(a, b);

    return 0;
}
```

# 90 : 문장의 문자수와 단어수
문제 : 문장을 입력받고, 문자수와 단어수를 파악해서 출력해라  
근데 이거 공백문자도 문자수에 포함되는거인지는 몰라서 일단 공백문자 포함된 개수로 셌다.

출력 : 
```
Enter sentence : I love C Language!!
문자 수 : 19    단어 수 : 4
```

소스 : 
```cpp
// 90 : 문장을 입력받으면 문자수와 단어수를 파악하는 프로그램을 포인터로 작성

#include <stdio.h>
#define MAX 100

int countLetter(char* s) {
    int count = 0;
    char* pCount = s;
    while (*pCount != '\n') {
        ++count;
        ++pCount;
    }

    return count;
}

int countWord(char* s) {
    int count = 0;
    char* pCount = s;

    if (*s != ' ') {
        ++count;
    }

    while (*pCount != '\0') {
        if (*pCount == ' ') {
            ++count;
        }
        ++pCount;
    }

    return count;
}

int main() {
    int letterCount, wordCount;
    char s[MAX];
    printf("Enter sentence : ");
    fgets(s, sizeof(s), stdin);

    letterCount = countLetter(s);
    wordCount = countWord(s);

    printf("문자 수 : %d\t단어 수 : %d\n", letterCount, wordCount);

    return 0;
}
```

# 91 : 문장을 단어단위로 거꾸로 출력
문제 : 문장을 입력받아 단어단위로 역순으로 출력해라.

출력 : 
```
Enter sentence : I love Computer Science !!!
!!! Science Computer love I
```

소스 : 
```cpp
// 91 : 문장을 받아서 단어 단위로 역순으로 출력하는 프로그램

#include <stdio.h>
#include <stdlib.h>
#define MAX 100     // 문장 최대 길이

// counts letter from sentence *s
// = strlen?
int countLetter(char* s) {
    int count = 0;
    char* pCount = s;
    while (*pCount != '\n') {
        ++count;
        ++pCount;
    }

    return count;
}

// 문장 속 단어들을 역순으로 출력하는 함수.
void reverse(char* s) {
    char* p = s;
    int space = 0;
    int len = countLetter(s);

    // (문장길이 + 1)짜리 배열에서 
    // 띄어쓰기와 마지막 종결문자가 있는 부분을 1로 마킹
    // 나머지는 0으로 마킹해두는 배열
    int* spacePos = (int*)calloc(len + 1, sizeof(int));

    for (size_t i = 0; i < len + 1; ++i) {
        if (*(p + i) == ' ' || *(p + i) == 10) {
            spacePos[i] = 1;
        }
    }

    // ex) I Love You\n
    //     01000010001
    // 위의 배열에서 처음으로 발견한 지점에서 "10"이 붙어 있으면 거기부터
    // 다음 1 나올때까지 한 단어 출력
    // 이렇게 역순으로 진행하고
    // 맨 처음 단어같은 경우 두번째 if문의 조건으로 걸러서 첫 단어도 출력하게함

    for (size_t i = len; i > 0; --i) {
        if (spacePos[i] == 0 && spacePos[i - 1] == 1) {
            int j = i;
            while (spacePos[j] != 1) {
                printf("%c", s[j]);
                ++j;
            }
            printf(" ");
        }
        if (i == 1 && !(spacePos[0] && spacePos[1])) {
            int j = i - 1;
            while (spacePos[j] != 1) {
                printf("%c", s[j]);
                ++j;
            }
            printf("\n");
        }
    }
}

int main() {
    char s[MAX];
    printf("Enter sentence : ");
    fgets(s, sizeof(s), stdin);

    reverse(s);

    return 0;
}
```

# 101 : 행맨 게임 (일단 coino.h 없이...) -> 구현 실패
본인은 맥-북을 쓰고있기 때문에 UNIX 시스템에서는 conio.h 따위는 쓸 수 업따...  
문제 : 행맨 게임을 만들어라. 15회 이내 도전으로 단어를 맞추면 우승이다. ~~정답은 concatenation 이다.~~

출력 : 
```
 count : 7
c o n c _ t _ n _ t i o n 
Enter character guess : 
(진행...)
 count : 23
Answer : concatenation
You Lose!!
```

소스 : 
```cpp
// 101 : 행맨 게임
// 15번 이하 횟수로 맞추면 우승, 단어를 추리할때마다 횟수 +1

#include <stdio.h>
#include <stdlib.h>

int main() {
    const char answer[] = "concatenation";
    int len = sizeof(answer) - 1;

    // 문자를 말했을 때 그 문자가 정답에 포함되어있으면
    // 해당 문자의 위치의 배열 값을 0에서 1로 바꿈
    int* hMan = (int*)calloc(len - 1, sizeof(int));

    int cnt = 0;
    int result = 0;

    while (result != len) {
        result = 0;
        for (size_t i = 0; i < len; ++i) {
            result += hMan[i];
        }

        printf("count : %d\n", cnt);

        if (result == len) {
            printf("Answer : %s\n", answer);
            if (cnt <= 15) {
                printf("You Win!!\n");
            } else {
                printf("You Lose!!\n");
            }
            return 0;
        }

        for (size_t i = 0; i < len; ++i) {
            if (!hMan[i]) {
                printf("_ ");
            } else {
                printf("%c ", answer[i]);
            }
        }


        printf("\n");
        printf("Enter character guess : ");
        char ch;
        ch = getchar();         // 일단 여기서 문자를 단 한개식 받지 못하고
        getchar();              // \n이 딸려와서 받는게 큰 문제이다....
        ++cnt;

        for (size_t i = 0; i < len; ++i) {
            if (answer[i] == ch && ch != '\0') {
                hMan[i] = 1;
            }
        }

        fflush(stdout);         // 출력 버퍼 플러시 한다는게 아직도 뭔 뜻인지 모르겠다...
        system("clear");
    }

    return 0;
}
```

# 106 : 파일 입출력 + 구조체
문제 : 106 : data.txt에서 정보를 불러와서 각 학생들의 두 점수의 합을 맨 마지막 열에 출력하고, 앞 열들은 data.txt랑 똑같이 출력해보자  
사실 구조체 써야되는지 모르겠다... 파일 IO 다 까먹어서 일단 인터넷 도움받아 구현함

data.txt 내용
```
1 Hong 30.3 40.5
2 Lim 28.3 37.5
3 Jang 32.5 77.3
4 Lee 67.2 39.9
5 Park 77.8 67.4
6 Jeon 22.4 37.5
```
출력 : 
```
Successfully read file!
1  Hong 30.3 40.5  70.8
2   Lim 28.3 37.5  65.8
3  Jang 32.5 77.3 109.8
4   Lee 67.2 39.9 107.1
5  Park 77.8 67.4 145.2
6  Jeon 22.4 37.5  59.9
```

소스 : 
```cpp
// 106 : data.txt에서 정보를 불러와서 각 학생들의 두 점수의
// 합을 맨 마지막 열에 출력하고, 앞 열들은 data.txt랑 똑같이 출력해보자

#include <stdio.h>
#include <stdlib.h>
#include <strings.h>

#define DATA "data.txt"
#define MAX 20

typedef struct _Student {
    int idx;
    char name[5];
    float score1;
    float score2;
} Student;

int main() {
    FILE* fp;
    fp = fopen(DATA, "r");

    if (fp == NULL) {
        printf("Error reading file!\n");
        exit(EXIT_FAILURE);
    } else {
        printf("Successfully read file!\n");
    }


    char buffer[MAX];

    // feof(): 파일 끝에 도착함 = nonzero 리턴
    //         아직 안 도착함   = 0 리턴
    while (!feof(fp)) {
        Student s;
        fscanf(fp, "%d %s %f %f", &s.idx, s.name, &s.score1, &s.score2);
        printf("%d %5s %.1f %.1f %5.1f\n",
        s.idx, s.name, s.score1, s.score2, s.score1 + s.score2);
    }

    fclose(fp);
    return 0;
}
```

# 대망의 연결리스트 (한방향 연결리스트) - 49 : 연결리스트와 난수
문제 풀 것 찾다가 연결리스트 다루는 게 있어서 잠깐 공부해서 풀어보았다. 생각보다 간단해서 재밌었다.

문제 : 연결리스트를 구현하고 30개의 난수를 리스트에 넣어 합계와 평균을 계산해라

출력 : 
```
HEAD - 79 - 13 - 61 - 70 - 23 - 4 - 50 - 35 - 32 - 93 - TAIL
Sum : 460 Avg : 46.0
```
소스 : 
```cpp
// 49 : 정수를 저장할 수 있는 연결리스트를 만들고
// 30개의 난수를 저장하고, 걔네들의 합과 평균을 구하시오.

#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define MAX 10

typedef struct _Node {
        int data;               // 리스트의 각 노드가 가진 데이터
        struct _Node* next;     // 다음 노드를 가르키는 부분
} Node;

// appends at the last node : 맨 뒤에 새로운 노드를 추가한다
void listAppend(Node* list, int data) {
    // 인수로 받은 리스트를 참조, 현위치를 나타내는 노드 포인터
    Node* current = list;

    // 다음 노드가 비어 있을 때까지 리스트를 따라 포인터를 한칸씩 이동
    while (current->next != NULL) {
        current = current->next;
    }

    // 그러고 나서 맨뒤에 새로운 노드 생성
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = data;
    newNode->next = NULL;

    // 현재위치의 노드 -> 새로운 노드 -> NULL
    // 이런 방법으로 노드를 연결함
    current->next = newNode;
}

// prints list
void listPrint(Node* list) {
    Node* current = list->next;
    
    printf("HEAD - ");

    // 노드가 비어있을 때 까지 리스트의 노드를 하나씩 프린트
    while (current != NULL) {
        printf("%d - ", current->data);
        current = current->next;
    }
    printf("TAIL\n");
}

// 리스트의 값들의 합
int listSum(Node* list) {
    int sum = 0;
    Node* current = list->next;
    while (current != NULL) {
        sum += current->data;
        current = current->next;
    }
    return sum;
}

// 리스트의 값들의 평균
double listAvg(Node* list) {
    int sum = listSum(list);

    int len = 0;
    Node* current = list->next;

    while (current != NULL) {
        ++len;
        current = current->next;
    }
    return (sum * 1.0 / len);
}

// free() list : 동적할당 할 때에는 free()를 잊지 말자... GC도 없는 C언어...
void listFree(Node* list) {
    Node* current = list;
    Node* temp;

    while (current != NULL) {
        temp = current->next;
        free(current);
        current = temp;
    }
}

int main() {
    Node* list1;
    list1 = (Node*)malloc(sizeof(Node));

    srand((unsigned int)time(NULL));

    for (size_t i = 0; i < MAX; ++i) {
        int rNum = rand() % 100;
        listAppend(list1, rNum);
    }

    listPrint(list1);

    printf("Sum : %d Avg : %3.1lf\n", listSum(list1), listAvg(list1));

    listFree(list1);
    return 0;
}
```

이 문제를 풀 때에는 헤더 노드를 딱히 만들지 않고 바로 리스트가 시작되게끔 구현했다.
> ![](https://imgur.com/4Lzh975.jpg)
> 가장 기본적인 연결 리스트는 이렇게 생겼다.

# 115 : 연결리스트를 활용한 다항식 문제
- 문제 1 : 다항식에 값을 대입했을 때 결과를 계산하는 함수를 만들고 출력하시오.  
- 문제 2 : 다항식을 더하는 소스코드를 제공했으니, 이를 활용하여 다항식을 곱하는 프로그램도 만들어보자.  

위에서 풀었던 것과는 다르게 헤더 노드와 그냥 노드를 구분해서 구조체를 만들었다.  
출력 :
```
<eq1 생성>
첫 항의 차수를 입력하세요 : 3
3차항의 계수 입력 : 4
2차항의 계수 입력 : -3
1차항의 계수 입력 : 2
0차항의 계수 입력 : 1
다항식 생성완료!

<eq2 생성>
첫 항의 차수를 입력하세요 : 2
2차항의 계수 입력 : -3
1차항의 계수 입력 : 5
0차항의 계수 입력 : 3
다항식 생성완료!

> eq1 = 4x^3 -3x^2 + 2x^1 + 1

> eq2 = -3x^2 + 5x^1 + 3

x = 2.00, eq1(x) = 25.00            // 문제 1에 해당
eq1 + eq2 = 4x^3 -6x^2 + 7x^1 + 4   // 문제 2에 주어진 소스코드

 // 주어진 소스를 활용해 다항식의 곱셈를 얻음
eq1 x eq2 = -12x^5 + 29x^4 -9x^3 -2x^2 + 11x^1 + 3 
```

소스 : 
```cpp
// 115-1 : 다항식에 값을 대입했을 때 결과를 계산하는 함수를 만들고 출력하시오
// 1번 문제도 연결리스트에 일단 다항식을 저장해야하나봄

#include <math.h>
#include <stdio.h>
#include <stdlib.h>

typedef struct _Node {
        int coef;  // 계수
        int exp;   // 지수
        struct _Node* next;
} Node;

// 지금보니 이름을 헤더랑 관련있는 거로 짓는게 나았을지도...
// 헤더 노드이다.
typedef struct _LinkedList {
        int size;
        Node* next;
} LinkedList;

// 연결리스트(의 헤더)를 생성해서 리턴한다.
LinkedList* listCreate() {
    LinkedList* list = (LinkedList*)malloc(sizeof(LinkedList));
    list->size = 0;
    list->next = NULL;

    return list;
}

// 다항식 (= 연결리스트)의 맨 뒤에 새로운 항(노드)를 추가한다
// 항이 추가될 다항식, 계수, 지수를 매개변수로 쓴다.
void polyAppend(LinkedList* list, int coef, int exp) {
    Node* newNode = (Node*)malloc(sizeof(Node));

    newNode->coef = coef;
    newNode->exp = exp;
    newNode->next = NULL;

    if (list->next == NULL) {
        list->next = newNode;
    } else {
        Node* cur = list->next;

        while (cur->next != NULL) {
            cur = cur->next;
        }
        cur->next = newNode;
    }

    // 새로운 항이 추가되었으니 항의 개수 + 1
    list->size++;
}

// 다항식을 이쁘게 뽑아주는 함수
void polyPrint(LinkedList* list) {
    Node* cur = list->next;

    while (cur != NULL) {
        if (cur->exp == 0) {
            printf("%d", cur->coef);
        } else {
            if (cur->coef == 1) {
                printf("x^%d", cur->exp);
            } else {
                printf("%dx^%d", cur->coef, cur->exp);
            }
        }
        cur = cur->next;
        if (cur != NULL) {
            if (cur->coef >= 0) {
                printf(" + ");
            } else {
                printf(" ");
            }
        }
    }
    printf("\n\n");
}

// 다항식에 x 값을 넣었을 때의 계산값 리턴
double polyCalc(LinkedList* list, double x) {
    double calc = 0;

    Node* cur = list->next;
    while (cur != NULL) {
        calc += cur->coef * pow(x, (double)cur->exp);
        cur = cur->next;
    }

    return calc;
}

// 다항식을 만들어줌
// polyAppend() 함수를 이용하여 다항식을 주루륵 만들어준다
// 리스트 생성과는 다른 의미다!
void equationCreate(LinkedList* list) {
    int firstExp;
    printf("첫 항의 차수를 입력하세요 : ");     // 첫 항의 차수가 곧 항의 개수
    scanf("%d", &firstExp);

    for (int i = firstExp; i >= 0; --i) {
        int coefInput;
        printf("%d차항의 계수 입력 : ", i);
        scanf("%d", &coefInput);

        polyAppend(list, coefInput, i);
    }
    printf("다항식 생성완료!\n\n");
}

// 두 다항식을 더하여 결과값의 다항식을 리턴한다.
LinkedList* polyAdd(LinkedList* polyA, LinkedList* polyB) {
    LinkedList* resultPoly = listCreate();

    Node* curA = polyA->next;
    Node* curB = polyB->next;

    int sum;
    while (curA && curB) {
        if (curA->exp == curB->exp) {
            sum = curA->coef + curB->coef;
            if (sum != 0) {
                polyAppend(resultPoly, sum, curA->exp);
            }
            curA = curA->next;
            curB = curB->next;
        } else if (curA->exp > curB->exp) {
            polyAppend(resultPoly, curA->coef, curA->exp);
            curA = curA->next;
        } else {
            polyAppend(resultPoly, curB->coef, curB->exp);
            curB = curB->next;
        }
    }

    while (curA != NULL) {
        polyAppend(resultPoly, curA->coef, curA->exp);
        curA = curA->next;
    }
    while (curB != NULL) {
        polyAppend(resultPoly, curB->coef, curB->exp);
        curB = curB->next;
    }

    return resultPoly;
}

// 두 다항식을 곱하여 결과값의 다항식을 리턴한다.
LinkedList* polyMult(LinkedList* polyA, LinkedList* polyB) {
    int multCnt = polyA->next->exp + 1;
    int coef, exp;
    LinkedList* temp[multCnt];
    LinkedList* result;

    Node* curA = polyA->next;
    Node* curB, *curTemp;

    for (size_t i = 0; i < multCnt; ++i) {
        temp[i] = listCreate();
        curB = polyB->next;
        curTemp = curB;
        for (size_t j = 0; j < polyB->size; ++j) {
            coef = curA->coef * curTemp->coef;
            exp = curA->exp + curTemp->exp;
            polyAppend(temp[i], coef, exp);

            curTemp = curTemp->next;
            
        }
        curA = curA->next;
    }

    result = temp[0];
    for (size_t i = 1; i < multCnt; ++i) {
        result = polyAdd(result, temp[i]);
    }

    return result;
}

// 다항식 free()
void listFree(LinkedList* list) {
    Node* temp;
    Node* cur = list->next;

    while (cur != NULL) {
        temp = cur->next;
        free(cur);
        cur = temp;
    }
}

int main() {
    LinkedList* eq1, *eq2;

    // 계산 결과를 반환하는 함수의 원리 덕분에
    // 이 둘은 리스트 생성함수를 안돌려도 됨
    LinkedList* sumEq, *multEq;

    eq1 = listCreate();
    eq2 = listCreate();

    printf("<eq1 생성>\n");
    equationCreate(eq1);
    printf("<eq2 생성>\n");
    equationCreate(eq2);

    printf("> eq1 = ");
    polyPrint(eq1);

    printf("> eq2 = ");
    polyPrint(eq2);

    double x = 2;
    printf("x = %.2lf, eq1(x) = %.2lf\n", x, polyCalc(eq1, x));

    sumEq = polyAdd(eq1, eq2);
    printf("eq1 + eq2 = ");
    polyPrint(sumEq);

    multEq = polyMult(eq1, eq2);
    printf("eq1 x eq2 = "); polyPrint(multEq);

    listFree(eq1);
    listFree(eq2);
    listFree(sumEq);
    listFree(multEq);
    return 0;
}
```
## 간략한 설명
> ![](https://imgur.com/55kZQu8.jpg)
> 본 문제에서 사용한 연결리스트는 이렇게 생겼다.

---

### polyAppend() 함수
![](https://imgur.com/gQ8snnG.jpg)
1. 입력받은 계수, 지수를 통해 새 노드를 일단 만든다.
    > 얘는 이미 다음 노드가 NULL을 향해있다.
2. 노드를 탐색해 나가며 마지막 노드를 찾는다
3. 마지막 노드가 새로 만든 노드를 향하게 해준다.
4. ???
5. PROFIT!!
> 위 문제에서는 만약에 빈 리스트라면 그냥 탐색없이 헤더가 바로 새 노드를 향하게끔 구현해 놓았다.

연결리스트를 구현하면 `Node* currentNode` 등의 표현으로 현재 위치를 알려주는 거를 만들어 사용한다. 마치 파일탐색기에서 방향키를 통해 폴더나 파일을 고르면 골라진 파일에 하이라이트가 되는 그런 느낌이다(?)

---

### polyAdd() 함수
다항식을 더할 때에는 차수가 같은거 끼리 더하는건 다들 알고 있을것이다.
다항식 두개를 매개변수로 쓰는 만큼 currentNode도 두개 만들어서 쓴다.
> ![](https://imgur.com/lng48VK.jpg)
> 그림이 조잡하지만 내가 잘 이해했기를 바란다...

---

### polyMult() 함수
![](https://imgur.com/Pi5U6fD.jpg)

다항식의 곱셈은
1. 분배법칙으로 일단 나눈걸 각 항별로 계산해주고
> 소스코드에서 해당 부분
> ```cpp
> int multCnt = polyA->next->exp + 1;
> int coef, exp;
> LinkedList* temp[multCnt];
> LinkedList* result;
> 
> Node* curA = polyA->next;
> Node* curB, *curTemp;
> 
> for (size_t i = 0; i < multCnt; ++i) {
>     // 위 그림에서 분배법칙 이후 계산된 항을 담기위한 배열
>     temp[i] = listCreate();       
> 
>     curB = polyB->next;
>     curTemp = curB;       
> 
>     for (size_t j = 0; j < polyB->size; ++j) {
>         coef = curA->coef * curTemp->coef;
>         exp = curA->exp + curTemp->exp;
>         polyAppend(temp[i], coef, exp);
> 
>         curTemp = curTemp->next;
>         }
>     curA = curA->next;
>     }
> ```
2. 계산된 것들을 다 더해주면
3. ???
4. PROFIT!!

`(3x + 2)(2x + 4)` 처럼 단순한 경우에는 2번 단계에서 `polyAdd()`를 한번만 호출하면 되지만, 그림의 아래부분처럼 여러개가 되버리는 경우에는 `polyAdd()`함수를 여러번 호출해야 한다. 하지만 앞에서부터 두개씩 묶어서 더해주면 문제없다.  

> 소스코드에서의 해당 부분 : 
> ```cpp
>     result = temp[0];
>     for (size_t i = 1; i < multCnt; ++i) {      // multCnt = 앞 항 개수
>         result = polyAdd(result, temp[i]);
>     }
> 
>     return result;
> ```