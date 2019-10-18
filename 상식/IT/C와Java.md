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

### 문자열

 문자열은 char 포인터 형식으로 사용한다. char *변수이름 = "문자열"; 문자열은 다음과 같이 포인터를 이용해서 저장한다. 포인터는 메모리 주소를 저장하므로 메모리 주소를 보고 문자열이 저장된 위치로 이동한다. 

``` c
char *s1 = "Hello";

printf("%p\n", s1);    // 00A5585: %p로 문자열이 저장된 메모리 주소 출력
```

![img](https://dojang.io/pluginfile.php/392/mod_page/content/33/unit39-1.png)

 문자열 리터럴이 변수 s1안에 저장되지 않고 (일반적인 포인터라면 s1은 변수의 주소, *s1은 변수의 값이 되어야 한다) 문자열 리터럴("Hello")이 있는 곳의 메모리 주소만 저장된다. 

 C언어의 문자열은 마지막에 항상 null 문자가 붙는다. 이는 문자열의 끝을 나타내어서 printf가 문자열을 출력할 때 null에서 출력을 끝낸다. 

![img](https://dojang.io/pluginfile.php/392/mod_page/content/33/unit39-2.png)

 문자열 리터럴이 있는 메모리 주소는 읽기 전용이기 때문에 문자열 포인터는 인덱스로 접근하여 문자를 할당할 수 없다.

``` c
#include <stdio.h>

int main()
{
    char *s1 = "Hello";    // 포인터에 문자열 Hello의 주소 저장
                           // Hello가 있는 메모리 주소는 읽기 전용

    s1[0] = 'A';           // 문자열 포인터의 인덱스 0에 문자 A를 할당
                           // 실행 에러

    printf("%c\n", s1[0]);

    return 0;
}
```

 문자열은 배열 형태로도 저장할 수 있다. 이 방식은 배열 요소 하나하나에 문자가 저장된다. 즉, 배열 안에 일렬로 나열된 문자가 모여 문자열을 이루게 된다. 여기에도 문자열의 맨 뒤에 null이 들어간다. 한 가지 주의할 점은 배열을 선언한 즉시 문자열로 초기화해야 한다는 것이다. 배열을 미리 선언하고 나중에 할당할 수는 없다.

``` c
#include <stdio.h>

int main()
{
    char s1[10] = "Hello";  // 크기가 10인 char형 배열을 선언하고 문자열 할당

    printf("%s\n", s1);     // Hello: %s로 문자열 출력

    return 0;
}
```

 문자열을 저장할 때 배열의 초소 크기는 글자수 + 1('\0')이다. 그리고 배열 형태의 문자열은 인덱스로 문자에 접근할 수 있다.

``` c
#define _CRT_SECURE_NO_WARNINGS // scanf 보안 경고로 인한 컴파일 에러 방지
#include <stdio.h>

int main()
{
    char s1[10];    // 크기가 10인 char형 배열을 선언

    printf("문자열을 입력하세요: ");
    scanf("%s", s1);     // 표준 입력을 받아서 배열 형태의 문자열에 저장

    printf("%s\n", s1);  // 문자열의 내용을 출력

    return 0;
}
```

 배열은 위 코드와 같이 문자열을 입력받을 수 있다. 다만 포인터는 위에서 사용했던 방식으로 하면 안된다. 포인터를 배열처럼 사용해야 한다.

``` c 
#define _CRT_SECURE_NO_WARNINGS     // scanf 보안 경고로 인한 컴파일 에러 방지
#include <stdio.h>
#include <stdlib.h>    // malloc, free 함수가 선언된 헤더 파일

int main()
{
    char *s1 = malloc(sizeof(char) * 10);    // char 10개 크기만큼 동적 메모리 할당

    printf("문자열을 입력하세요: ");
    scanf("%s", s1);      // 표준 입력을 받아서 메모리가 할당된 문자열 포인터에 저장

    printf("%s\n", s1);   // 문자열의 내용을 출력

    free(s1);    // 동적 메모리 해제

    return 0;
}
```

 

### 구조체

 구조체는 다양한 방법으로 만들 수 있다.

``` c
struct Person {   // 구조체 정의
    char name[20];        // 구조체 멤버 1
    int age;              // 구조체 멤버 2
    char address[100];    // 구조체 멤버 3
};

struct Person p1;    // 구조체 변수 선언

struct Person {    // 구조체 정의
    char name[20];        // 구조체 멤버 1
    int age;              // 구조체 멤버 2
    char address[100];    // 구조체 멤버 3
} p1;              // 구조체를 정의하는 동시에 변수 p1 선언

typedef struct _Person {    // 구조체 이름은 _Person
    char name[20];             // 구조체 멤버 1
    int age;                   // 구조체 멤버 2
    char address[100];         // 구조체 멤버 3
} Person;                   // typedef를 사용하여 구조체 별칭을 Person으로 정의

Person p1;    // 구조체 별칭 Person으로 변수 선언
struct _Person p1; // 구조체 변수 선언  위, 아래 방법 둘 다 가능하다.

typedef struct {    // 구조체 이름이 없는 익명 구조체
    char name[20];        // 구조체 멤버 1
    int age;              // 구조체 멤버 2
    char address[100];    // 구조체 멤버 3
} Person;           // typedef를 사용하여 구조체 별칭을 Person으로 정의

Person p1;    // 구조체 별칭 Person으로 변수 선언

```

 구조체 변수를 일일이 선언해서 사용하는 것보다는 포인터에 메모리를 할당해서 사용하는 편이 효율적이다. 

``` c
#define _CRT_SECURE_NO_WARNINGS    // strcpy 보안 경고로 인한 컴파일 에러 방지
#include <stdio.h>
#include <string.h>    // strcpy 함수가 선언된 헤더 파일
#include <stdlib.h>    // malloc, free 함수가 선언된 헤더 파일

struct Person {    // 구조체 정의
    char name[20];        // 구조체 멤버 1
    int age;              // 구조체 멤버 2
    char address[100];    // 구조체 멤버 3
};

int main()
{
    struct Person *p1 = malloc(sizeof(struct Person));    // 구조체 포인터 선언, 메모리 할당

    // 화살표 연산자로 구조체 멤버에 접근하여 값 할당
    strcpy(p1->name, "홍길동");
    p1->age = 30;
    strcpy(p1->address, "서울시 용산구 한남동");

    // 화살표 연산자로 구조체 멤버에 접근하여 값 출력
    printf("이름: %s\n", p1->name);       // 홍길동     (*p1).name 과 같은 의미이다.
    printf("나이: %d\n", p1->age);        // 30
    printf("주소: %s\n", p1->address);    // 서울시 용산구 한남동

    free(p1);    // 동적 메모리 해제

    return 0;
}
```

 구조체의 멤버가 포인터일 때 역참조하는 방법은 맨 앞에 *을 붙이는 것이다. 구조체 변수 앞에 *가 붙어있더라도 멤버의 역참조이지 구조체 변수의 역참조가 아님을 알아야 한다.

- *구조체변수.멤버
- *구조체포인터->멤버

 역참조한 것을 괄호로 묶으면 구조체 변수를 역참조한 뒤 멤버에 접근한다는 뜻이다. 

- (* 구조체포인터).멤버 == 구조체변수.멤버
- *( * 구조체포인터).멤커 == * 구조체변수. 멤버

``` c
#include <stdio.h>
#include <stdlib.h>

struct Data {
    char c1;
    int *numPtr;    // 포인터
};

int main()
{
    int num1 = 10;
    struct Data d1;    // 구조체 변수
    struct Data *d2 = malloc(sizeof(struct Data));    // 구조체 포인터에 메모리 할당

    d1.numPtr = &num1;
    d2->numPtr = &num1;

    printf("%d\n", *d1.numPtr);     // 10: 구조체의 멤버를 역참조
    printf("%d\n", *d2->numPtr);    // 10: 구조체 포인터의 멤버를 역참조

    d2->c1 = 'a';
    printf("%c\n", (*d2).c1);      //  a: 구조체 포인터를 역참조하여 c1에 접근
                                   // d2->c1과 같음
    printf("%d\n", *(*d2).numPtr); // 10: 구조체 포인터를 역참조하여 numPtr에 접근한 뒤 다시 역참조
                                   // *d2->numPtr과 같음

    free(d2);

    return 0;
}
```

``` c
#include <stdio.h>

struct abc {
	int a;
	int b;
	char c;
};

struct cba {
	char a;
	int c;
	char b;
};

struct bca {
	char a;
	char b;
	int c;
};

int main(void) {
	// your code goes here
	printf("%d", sizeof(struct abc)); // 12
    printf("%d\n", sizeof(struct cba)); // 12
    printf("%d\n", sizeof(struct bca)); // 8
    
	printf("%d, %d, %d", offsetof(struct bca, a), offsetof(struct bca, c), offsetof(struct bca, b));
	
	return 0;
}

```

![img](https://dojang.io/pluginfile.php/495/mod_page/content/25/unit51-1.png)

### 형변환

  자료형 뒤에 포인터를 나타내는 *를 붙여주고 괄호로 묶어줘야 한다.

- (자료형 *)포인터

 pointer to int 였던 변수를 pointer to char로 바꿀 경우, 역참조 시 4바이트가 아니라 1바이트를 가져온다. 저장된 메모리 주소는 같지만 자료형에 따라 역참조했을 때 값을 가져오는 크기가 결정된다. 크기가 작은 메모리 공간을 할당한 뒤 큰 자료형의 포인터로 역참조하면 의도치 않은 값을 가져올 수도 있다.

``` c
#include <stdio.h>
#include <stdlib.h>    // malloc, free 함수가 선언된 헤더 파일

int main()
{
    int *numPtr = malloc(sizeof(int));    // 4바이트만큼 메모리 할당
    char *cPtr;

    *numPtr = 0x12345678;

    cPtr = (char *)numPtr;     // int 포인터 numPtr을 char 포인터로 변환. 메모리 주소만 저장됨

    printf("0x%x\n", *cPtr);   // 0x78: 낮은 자릿수 1바이트를 가져오므로 0x78

    free(numPtr);    // 동적 메모리 해제

    return 0;
}
```

![img](https://dojang.io/pluginfile.php/558/mod_page/content/26/unit58-2.png)

![img](https://dojang.io/pluginfile.php/558/mod_page/content/26/unit58-3.png)

 void 포인터는 자료형이 정해져 있지 않으므로 역참조 연산을 할 수 없다. 하지만 void 포인터를 다른 자료형으로 변환하면 역참조를 할 수 있다.

``` c
#include <stdio.h>

int main()
{
    int num1 = 10;
    float num2 = 3.5f;
    char c1 = 'a';
    void *ptr;

    ptr = &num1;    // num1의 메모리 주소를 void 포인터 ptr에 저장
    // printf("%d\n", *ptr);         // 컴파일 에러
    printf("%d\n", *(int *)ptr);     // 10: void 포인터를 int 포인터로 변환한 뒤 역참조

    ptr = &num2;    // num2의 메모리 주소를 void 포인터 ptr에 저장
    // printf("%f\n", *ptr);         // 컴파일 에러
    printf("%f\n", *(float *)ptr);   // 3.500000: void 포인터를 float 포인터로 변환한 뒤 역참조

    ptr = &c1;      // c1의 메모리 주소를 void 포인터 ptr에 저장
    // printf("%c\n", *ptr);         // 컴파일 에러
    printf("%c\n", *(char *)ptr);    // a: void 포인터를 char 포인터로 변환한 뒤 역참조

    return 0;
}
```

 자료형 변환은 구조체 포인터를 변환할 때 주로 쓰인다. 이 때는 struct와 구조체 이름 뒤에 *을 붙여주고 괄호로 묶어줘야 한다.

- (struct 구조체이름 *) 포인터이름;
- ((struct 구조체이름 *)포인터)->멤버

![img](https://dojang.io/pluginfile.php/560/mod_page/content/24/unit58-4.png)

typedef로 구조체 포인터의 별칭을 정할 수도 있다.

``` c
#include <stdio.h>
#include <stdlib.h>    // malloc, free 함수가 선언된 헤더 파일

typedef struct _Data {
    char c1;
    int num1;
} Data, *PData;     // 구조체 별칭, 구조체 포인터 별칭 정의

int main()
{
    PData d1 = malloc(sizeof(Data));    // 구조체 포인터 별칭으로 포인터 선언
    void *ptr;   // void 포인터 선언

    d1->c1 = 'a';
    d1->num1 = 10;

    ptr = d1;    // void 포인터에 d1 할당. 포인터 자료형이 달라도 컴파일 경고가 발생하지 않음.

    printf("%c\n", ((Data *)ptr)->c1);     // 'a' : 구조체 별칭의 포인터로 변환
    printf("%d\n", ((PData)ptr)->num1);    // 10  : 구조체 포인터 별칭으로 변환

    free(d1);    // 동적 메모리 해제

    return 0;
}
```

### 함수



## Java

![자바 자료형에 대한 이미지 검색결과](http://mblogthumb3.phinf.naver.net/20160504_170/klp0712_14622884175091UfSf_PNG/1.PNG?type=w800)