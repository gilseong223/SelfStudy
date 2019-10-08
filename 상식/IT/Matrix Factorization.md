# Matrix Factorization

[TOC]

## 들어가기에 앞서

 ![570409849499](https://i.imgur.com/Zg7lqZM.png)

 세 가지의 Matrix에서 무엇이 가장 현실에 가까울까. 첫번째 표를 보면 모두가 같은 취향을 가지고 있다. 세번째 표는 모든 유저가 다 다른 취향을 가지고 다른 특성을 가지고 있다. 두번째 표가 가장 현실적이다.

 두번째 표는 랜덤한 것처럼 보이지만 사실은 영화간, 유저간 Dependency가 있다. 1번 유저와 3번 유저를 보면 완전히 취향이 같은 것을 볼 수 있다. 이런 경우 1번 유저와 3번 유저는 유사하다고 할 수 있다. 1번 영화와 4번 영화 역시 유사하다.

 유저 2,3,4의 유사성이 보이는지? 2 + 3 = 4 가 성립한다. 2번 유저가 코미디를 좋아하고 3번 유저가 액션을 좋아한다면 4번 유저는 코미디와 액션을 모두 좋아한다고 할 수 있다.

 2,3,5번 영화의 Dependency는 M5 = Avg(M2, M3) 이다. 

 여기서 말하고자 하는 바는 테이블 안에 Dependency가 있다는 것. 

![1570410167281](https://i.imgur.com/VUYIWZ9.png)

![1570410201456](https://i.imgur.com/xbv8kCX.png)

## How do we figure out all these dependencies?

 답은 Matrix Factorization이다. 작은 Feature들을 모아 큰 테이블을 만든다.

 유저1은 코미디를 좋아하고, 액션을 좋아하지 않는다. 영화1은 코미디가 3 정도고, 액션이 1이다. 그러면 유저1은 코미디에 대한 점수를 3점 주고 액션에 대한 점수를 주지 않는다. 그래서 유저1은 3점을 줄 것이다.

![1570410489755](https://i.imgur.com/o6TQJVO.png)

![1570410525464](https://i.imgur.com/jwcnpz3.png)

유저 1과 3은 같은 취향을 가지고 있는 것을 알 수 있다. 그래서 모든 영화에 대해 같은 점수를 주었다. 영화 1과 4도 같은 점수. 아까 살펴보았던 특징들이 모두 있다.

## Storage

![1570410768474](https://i.imgur.com/JNu7ZzK.png)

 만약 유저가 2000명이고 영화가 1000개라면 2,000,000개의 테이블을 보관해야 한다. 하지만 MF를 사용하면 그럴 필요하 없다. 영화 1000개에 대해 Feature 개수만큼만 곱하면 되고, 유저 2000명에 대해 Feature개수 만큼 곱하면 된다. 그리고 두 수를 더하면 된다. 그리고 한 유저에 대한 예측은 행렬곱을 통해 하면 된다.

## In Reality, How can we find the right Factorization?

### Gradient Decent

 처음에 random value로 시작해서 점점 정답에 근접하도록 값을 수정해나간다. 

![1570411133086](https://i.imgur.com/X9dOvbL.png)

이러한 경우, 1.44보다 큰 수가 나와야 하니까 F1, F2를 조금씩 증가시켜야 한다. 점점 에러 수정해가면서 완성된 표를 만든다.

## How to use this?

 ![1570411461765](https://i.imgur.com/7Gv1FvD.png)

사실 유저-영화 테이블은 Sparse Matrix이다. 그래서 행렬곱으로 이걸 채움으로써 예측이 완료된다. 



https://github.com/yanneta/pytorch-tutorials/blob/master/collaborative-filtering-nn.ipynb