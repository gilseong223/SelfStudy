# C & Java

[TOC]

## C언어

![img](https://s3-ap-northeast-2.amazonaws.com/opentutorials-user-file/module/3921/9904.png)



![](https://s3-ap-northeast-2.amazonaws.com/opentutorials-user-file/module/3921/9987.PNG)

### 포인터

 메모리 주소는 포인터 변수에 저장한다. 

- 자료형 * 포인터이름 = &변수 ;

``` c
#include <stdio.h>

int main()
{
    int *numPtr;      // 포인터 변수 선언
    int num1 = 10;    // int형 변수를 선언하고 10 저장

    numPtr = &num1;   // num1의 메모리 주소를 포인터 변수에 저장

    printf("%p\n", numPtr);    // 0055FC24: 포인터 변수 numPtr의 값 출력
                               // 컴퓨터마다, 실행할 때마다 달라짐
    printf("%p\n", &num1);     // 0055FC24: 변수 num1의 메모리 주소 출력
                               // 컴퓨터마다, 실행할 때마다 달라짐

    return 0;
}
```

 포인터 변수에는 메모리 주소가 저장되어 있다. 이 때, 메모리 주소가 있는 곳으로 이동해서 값을 가져오고 싶다면 역참조 연산자 `*`를 사용하면 된다.

``` c
#include <stdio.h>

int main()
{
    int *numPtr;      // 포인터 변수 선언
    int num1 = 10;    // 정수형 변수를 선언하고 10 저장

    numPtr = &num1;   // num1의 메모리 주소를 포인터 변수에 저장

    printf("%d\n", *numPtr);    // 10: 역참조 연산자로 num1의 메모리 주소에 접근하여 값을 가져옴

    return 0;
}
```

 역참조연산자는 자료형을 바꾸는 효과를 낸다. int *numPtr에서 numPtr의 자료형은 'pointer to int'이지만 *numPtr은 'int'형 이다. 즉, *numPtr의 사이즈는 int의 사이즈와 같이 4bytes가 된다. long long이나 float의 포인터도 똑같이 해당 자료형의 사이즈와 같다. 다만 sizeof(numPtr)인 경우는 자료형과 상관없이 32bit 운영체제에서 4byte가 된다.

 주소 연산자도 자료형을 바꿔준다. int num1이 있을 때, numPtr = &num1을 할 경우 자료형이 일치하게 된다. 

 포인터의 포인터도 가능하다. 포인터의 메모리 주소를 저장하는 포인터의 포인터를 선언할 수 있다.

``` c
#include <stdio.h>

int main()
{
    int *numPtr1;     // 단일 포인터 선언
    int **numPtr2;    // 이중 포인터 선언
    int num1 = 10;

    numPtr1 = &num1;    // num1의 메모리 주소 저장 

    numPtr2 = &numPtr1; // numPtr1의 메모리 주소 저장

    printf("%d\n", **numPtr2);    // 20: 포인터를 두 번 역참조하여 num1의 메모리 주소에 접근

    return 0;
}
```

### 메모리

 포인터에 원하는 만큼 메모리 공간을 할당받아 사용하는 방법을 알아보자. 메모리는 malloc -> 사용 -> free 패턴으로 사용한다.

- 포인터 = malloc(크기);
  - void *malloc(size_t_Size);
  - 성공하면 메모리 주소 반환, 실패하면 null 반환

``` c
#include <stdio.h>
#include <stdlib.h>    // malloc, free 함수가 선언된 헤더 파일

int main()
{
    int num1 = 20;    // int형 변수 선언
    int *numPtr1;     // int형 포인터 선언

    numPtr1 = &num1;  // num1의 메모리 주소를 구하여 numPtr에 할당

    int *numPtr2;     // int형 포인터 선언

    numPtr2 = malloc(sizeof(int));    // int의 크기 4바이트만큼 동적 메모리 할당

    printf("%p\n", numPtr1);    // 006BFA60: 변수 num1의 메모리 주소 출력
                                // 컴퓨터마다, 실행할 때마다 달라짐

    printf("%p\n", numPtr2);     // 009659F0: 새로 할당된 메모리의 주소 출력
                                // 컴퓨터마다, 실행할 때마다 달라짐

    free(numPtr2);    // 동적으로 할당한 메모리 해제

    return 0;
}
```

 원하는 시점에 원하는 만큼 메모리를 할당할 수 있다. 이를 동적 메모리 할당이라고 한다. numPtr1에는 일반 변수의 메모리 주소를 할당했고, numPtr2에는 malloc 함수로 메모리를 할당했다. 변수는 스택에 생성되며 malloc 함수는 힙 부분의 메모리를 사용한다.

``` c
free(numPtr2);    // 동적으로 할당한 메모리 해제
```

 메모리 해제는 선택이 아닌 필수이다. 메모리를 해제하지 않아 메모리 사용량이 증가하는 현상을 메모리 누수라고 한다.

  memset(배열, 초기화할 값, sizeof(배열)) 사용하면 배열 초기화 됨.

 null pointer는 메모리가 할당되지 않은 포인터이다. 

``` c
#include <stdio.h>

int main()
{
    int *numPtr1 = NULL;    // 포인터에 NULL 저장

    printf("%p\n", numPtr1);    // 00000000

    return 0;
}
```

 배열은 주솟값이기 때문에 포인터에 넣을 수 있다. 

``` c
#include <stdio.h>

int main()
{
    int numArr[10] = { 11, 22, 33, 44, 55, 66, 77, 88, 99, 110 };    // 크기가 10인 int형 배열

    int *numPtr = numArr;       // 포인터에 int형 배열을 할당

    printf("%d\n", *numPtr);    // 11: 배열의 주소가 들어있는 포인터를 역참조하면 배열의 
                                // 첫 번째 요소에 접근

    printf("%d\n", *numArr);    // 11: 배열 자체를 역참조해도 배열의 첫 번째 요소에 접근

    printf("%d\n", numPtr[5]);  // 66: 배열의 주소가 들어있는 포인터는 인덱스로 접근할 수 있음

    printf("%d\n", sizeof(numArr));    // 40: sizeof로 배열의 크기를 구하면 배열이 메모리에 
                                       // 차지하는 공간이 출력됨

    printf("%d\n", sizeof(numPtr));    // 4 : sizeof로 배열의 주소가 들어있는 포인터의 크기를 
                                       // 구하면 포인터의 크기가 출력됨(64비트라면 8)

    return 0;
}
```

 numPtr[5]를 해도 numArr[5]와 같은 값이 나온다.

 1차원 배열은 단일 포인터에 넣을 수 있지만, 2차원 배열은 이중 포인터에 넣을 수 없다. 포인터를 선언할 때 *과 포인터 이름을 괄호로 묶어준 뒤 가로 크기를 []로 지정한다.

``` c
#include <stdio.h>

int main()
{
    int numArr[3][4] = {    // 세로 3, 가로 4 크기의 int형 2차원 배열 선언
        { 11, 22, 33, 44 },
        { 55, 66, 77, 88 },
        { 99, 110, 121, 132 }
    };

    int (*numPtr)[4] = numArr;

    printf("%p\n", *numPtr); // 002BFE5C: 2차원 배열 포인터를 역참조하면 세로 첫 번째의 주소가 나옴
                             // 컴퓨터마다, 실행할 때마다 달라짐

    printf("%p\n", *numArr); // 002BFE5C: 2차원 배열을 역참조하면 세로 첫 번째의 주소가 나옴
                             // 컴퓨터마다, 실행할 때마다 달라짐

    printf("%d\n", numPtr[2][1]);    // 110: 2차원 배열 포인터는 인덱스로 접근할 수 있음

    printf("%d\n", sizeof(numArr));  // 48: sizeof로 2차원 배열의 크기를 구하면 배열이 메모리에 
                                     // 차지하는 공간이 출력됨

    printf("%d\n", sizeof(numPtr));  // 4 : sizeof로 2차원 배열 포인터의 크기를 
                                     // 구하면 포인터의 크기가 출력됨(64비트라면 8)

    return 0;
}
```

 배열은 메모리 해제를 시키지 않아도 되지만, 포인터는 해제해줘야 한다.

``` c
int numArr[10];                           // int형 요소 10개를 가진 배열 생성
int *numPtr = malloc(sizeof(int) * 10);   // int 10개 크기만큼 메모리 할당

numArr[0] = 10;    // 배열을 인덱스로 접근하여 값 할당
numPtr[0] = 10;    // 포인터를 인덱스로 접근하여 값 할당

free(numPtr);   // 메모리 해제
```

 2차원 배열을 동적으로 할당하기 위해서는 다음과 같은 방법을 써야 한다.

- 자료형 **포인터이름 = malloc(sizeof(자료형 *) * 세로크기);
- 반복문으로 반복하면서 포인터이름[i] = malloc(sizeof(자료형)*가로크기)
- 해제 시, 반복문으로 반복하면서 free(포인터이름[i]);
- free(포인터이름); 으로 해제 완료.

``` c
#include <stdio.h>
#include <stdlib.h>    // malloc, free 함수가 선언된 헤더 파일

int main()
{
    int **m = malloc(sizeof(int *) * 3);   // 이중 포인터에 (int 포인터 크기 * 세로 크기)만큼
                                           // 동적 메모리 할당. 배열의 세로

    for (int i = 0; i < 3; i++)            // 세로 크기만큼 반복
    {
        m[i] = malloc(sizeof(int) * 4);    // (int 크기 * 가로 크기)만큼 동적 메모리 할당.
                                           // 배열의 가로
    }

    m[0][0] = 1;    // 세로 인덱스 0, 가로 인덱스 0인 요소에 값 할당
    m[2][0] = 5;    // 세로 인덱스 2, 가로 인덱스 0인 요소에 값 할당
    m[2][3] = 2;    // 세로 인덱스 2, 가로 인덱스 3인 요소에 값 할당

    printf("%d\n", m[0][0]);    // 1: 세로 인덱스 0, 가로 인덱스 0인 요소의 값 출력
    printf("%d\n", m[2][0]);    // 5: 세로 인덱스 2, 가로 인덱스 0인 요소의 값 출력
    printf("%d\n", m[2][3]);    // 2: 세로 인덱스 2, 가로 인덱스 3인 요소의 값 출력

    for (int i = 0; i < 3; i++)    // 세로 크기만큼 반복
    {
        free(m[i]);                // 2차원 배열의 가로 공간 메모리 해제
    }

    free(m);    // 2차원 배열의 세로 공간 메모리 해제

    return 0;
}
```



## Java

![자바 자료형에 대한 이미지 검색결과](http://mblogthumb3.phinf.naver.net/20160504_170/klp0712_14622884175091UfSf_PNG/1.PNG?type=w800)