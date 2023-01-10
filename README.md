2291012 남윤혁  
이 글은 편하게 깃허브에서도 읽어볼수 있습니다.  
[깃허브 링크](https://github.com/nyhryan/coding-camp-2022)

# 코딩캠프 2023 첫날 (2023.01.09)
오후에 c 언어 2단계 문제 조금 풀다가 저녁먹고 끝.  
121 번 동적 계획법 알고리즘 문제(행렬 곱셈 문제) 풀다가 전혀 안되서 인터넷에서 찾아봤는데 그것도 이해 안되서 내일 다시 공부해야 할 것 같음.  
아래는 오늘 푼 문제들 (모두 C언어 2단계임)

# 01 : 1부터 n까지의 합에서 짝수만 더하는 재귀함수
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
목표 : 
```
1 = 1
1 + (1 + 2) = 4
1 + (1 + 2 + 3) = 10
...
10까지 출력하기
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
            // 1 = 1 출력
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
백만원을 연 이율 6.4%로 3년간 복리로 예치했을때의 3년뒤 총 금액은?  
공식 : (최초 예금액) * (1 + 이율) ^ 예치기간 = 총금액

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
# 05 : 두 점을 지나는 직선의 기울기와 y절편
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
# 06 : 두 행렬의 합
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
#include <stdio.h>

#define ROW 4
#define COL 3

// 고정크기 배열은 이중포인터가 아니라서 그냥 1중(?) 포인터로 받음
void addArray(int* a, int* b, int* result) {
    for (int i = 0; i < ROW * COL; ++i) {
        *(result + i) = *(a + i) + *(b + i);
    }
}

void printArr(int* arr) {
    for (int i = 0; i < ROW * COL; ++i) {
        printf("%d ", *(arr + i));
        if (i % COL == COL - 1) {
            printf("\n");
        }
    }
}

int main() {
    int a[ROW][COL] = {{1, 2, 3}, {2, 3, 4}, {3, 4, 5}, {4, 5, 6}};

    int b[ROW][COL] = {{1, 1, 1}, {0, 0, 0}, {3, 3, 3}, {4, 3, 2}};

    int result[ROW][COL] = {
        0,
    };

    printf("a = \n");
    printArr(a);               // 배열 넘길땐 & 필요 없음, 경고 짜증나면 (int*)로 캐스팅
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
출력 : 
```
English Highscore : John (100)
Math Highscore : Park (90)
```
```cpp
// 07: 학생들의 이름과 영어,수학 성적으로 구성된 구조체를 만들고
// 그 중 최고 성적을 받은 학생을 출력

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
                } else {
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
# 11 : 8, 16진수 재귀함수로 출력하기
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
공식화 시키면 위와 같다. `m[i][j] = 0 (i == j)` 인 이유는 1번째 배열에서 1번째 배열 까지 곱하는 횟수는 0번이기 때문인 것과 같다.

## 일단 해보기
![](https://imgur.com/xWZbkSE.jpg)
이 문제는 bottom-up 방식으로 푼다. 작은 문제로 위로 올라가면서 큰 문제를 푸는 뭐 그런거 같았던듯. 
![](https://imgur.com/ieVigfR.jpeg)

우리가 알고 싶은 `m[1][5]`를 알기 위해서는 `m[1][4]` 혹은 `m[2][5]`가 필요하다.  
이런 식으로 계속 내려가다 보면 `m[1][2], m[2][3]...`을 구해놓으면 결국에는 m[1][5]를 푸는데 사용할 수 있다. 
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
>❗️틀린 정보를 포함하고 있을 수도 있습니다...❗️

## 아래에서 다시 설명함. 여기는 아마도 틀린 말 하고 있을 듯
동적할당된 2차원배열을 []를 안쓰고 그리고 반복문을 하나만 돌려서 출력하는 함수를 만들다가 막혀서 질문을 많이하다가 알게된 것을 정리한다.
> 반복문 한개로는 절대 안되는 것 같다??... (사실 된다... 고정 크기 배열만) ~~그럼 안되는거잖아~~ㅖ


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

의문이 든건 배열의 9개의 연속된 주소값이라면 `**(arr1 + i)`를 통해 i를 1씩 증가하면서 아홉번 출력하면 9개 요소를 전부 출력할 수 있지 않나 인데...
> 사실은 이 표현은 틀린거다... 아래 참고  

돌려보면 **처음 세번은 각 행의 첫번째 요소만 출력되고 4번째 이후로는 쓰레기값이 나온다.**  
해결법은 반복문을 두번 돌려서
```cpp
for (int i = 0; i < 3; ++i) {
    for (int j = 0; j < 3; ++j) {
        printf("%d ", *(*arr1 + i) + j);
                      // --------   --
                      //i 번째 행   j번째 요소
    }
    printf("\n");
}
```
의 형식으로 돌리면 된다. ` *(*arr1 + i) + j)`라는 표현이 많이 괴랄하므로 그냥 `arr1[i][j]`를 이용하도록 하자(?)  
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

이런 방식으로 진행되어 반복문 하나로 배열을 출력할 수 있다.  
~~더럽게 복잡하니깐 그냥 편하게 for문 두번 쓰자.~~
### 참고 : 2차원배열을 함수인수로 넘기려면...
> 이런식으로 매개뱐수를 `array는 여러개의 COLUMN개짜리 배열을 묶어둔 것의 포인터`로 넘기던가 ~~뭔 소리야~~  
> 즉 `여러개의 COLUMN개의 1차원배열을 묶어둔 것중의 첫번째 배열의 주소 = 2차원배열의 맨 처음 요소의 주소`을 뜻하는 듯?
> ```cpp
> TYPE f(int (*array)[COLUMN]){...};
> ```
> 아래 방식으로 적는다. **위의 방법이 정석**이고,  
> 아래의 방법은 표기만 배열같은 것이지, 위의 표기와 같은 의미이다. 어짜피 배열을 넣어서 넘긴다 하더라도 컴파일러가 보면 배열은 곧 배열의 첫번째 요소를 가르키는 포인터와 같아서 이나저나 같다.
> ```cpp
> // ROW는 안써도 됨
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
> 3. `*(*(arr + i) + j)` : 위 2번 주소의 값

정도 되시겠다. ~~그니깐 제발 편하게 arr[i][j] 쓰자~~