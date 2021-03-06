# Multimedia Networking

[TOC]

## Multimedia

 오디오 혹은 비디오.

### Audio

 아날로그 시그널을 디지털로 저장. 연속적인 아날로그 시그널을 디지털화 하기 위해 샘플링 한다. 전화의 경우 초당 8000번의 샘플링이 있어야 한다. 샘플링에 따라 음질이 차이난다. 샘플링 할 때 몇 비트로 할 것이냐. 이러한 것들에 따라 bps가 정해진다.

스트리밍으로 mp3 음악을 듣고 싶다. mp3는 128kbps로 인코딩 되어 있다. 내 컴퓨터의 통신 속도가 이 인코딩 속도보다 높아야 끊김 없이 노래를 들을 수 있다.

### Video

 이미지의 연속이다. 초당 24개의 프레임이 들어있다. 각 이미지들은 그대로 저장되면 용량이 너무 크기 때문에, 중복되는 것은 압축해서 표현한다. 파란색이 연속해서 몇 번 반복된다 와 같은 방법으로 압축해서 저장한다. 이러한 것이 인코딩. 손실이 있을 수 있다. 하지만 용량을 적게 쓰기 때문에 사용함. mpeg2, mpeg4 등.

### Streaming Stored Video & Audio

 서버에서 초당 3mbps 보낸다고 했을 때, 실제 우리가 받게되는 데이터의 딜레이는 계속 변화한다. 미디어 플레이어에서 영상을 시작하는 시점을 조금 늦게 실행함. player delay. 그래서 어느정도 버퍼를 채운 다음에 보여준다.

#### Client-side buffering

 Playout buffering이 너무 길면 사용자는 많이 기다려야 함. 너무 짧으면 바로 볼 수는 있지만 중간에 끊겨서 기다려야할 수도 있다. 영상 스트리밍을 제공하는 서비스를 만들었다. 애플리케이션을 만든 것이니까 트랜스포트 레이어의 기능을 써야한다. TCP냐 UDP냐 사용해야 한다. TCP로 사용. 멀티미디어 콘텐츠는 각자의 인코딩 rate(bps)를 가진다. 3Mbps이기 때문에 무조건 3Mbps의 통신 속도는 있어야 플레이어에서 동작한다. 이런 데이터를 서버에서 클라이언트로 전달해야 한다. 

 UDP를 쓰면 서버에서 온전히 초당 3Mbps로 쏟아부을 수 있다. 네트워크 상태가 안좋아서 1Mbps라면 나는 제대로 방출하지만 클라이언트는 제대로 못 받는다.

 TCP를 쓰면 3Mbps로 내려 보냈다. 그런데 실제로 나가는 속도는 TCP가 조절한다. TCP가 3Mbps로 안보내주면 문제가 생김. 멀티미디어는 인코딩 레이트에 대한 requirement가 있기 때문에 TCP든 UDP든 문제가 생긴다.

 유튜브는 어떻게 해결했나. 어플리케이션 레이어에서 다른 테크닉을 사용함. 바로 대쉬.

#### DASH(Dynamic, Adaptive Streaming over HTTP)

 동적으로 적응하면서 환경에 적응하는 스트리밍을 해보겠다. HTTP를 사용해서. 결국 TCP를 사용한다. 

 영상을 원본 그대로 저장하는 것이 아니라 작은 조각으로 나눈다. chunk로 나눈다. 10초 짜리의 작은 청크로 나눔. 청크들을 각각 다른 버전으로 인코딩 한다. 1kbps, 2kbps, 1mbps, 5mbps 등등으로. 청크 1번이 1kbps, 2kbps.. 으로 저장해둔 것이다. 모든 청크에 대해 이렇게 해놓는다. 각 청크의 어느 속도가 저장되어 있는 URL을 테이블에 저장해둔다. 사용자가 영상을 보려고 할 때, 서버는 사용자에게 이 테이블을 준다. 사용자는 1번부터 그냥 재생한다. 영상이 잘 받아지고 있으면 좀 더 화질 좋은 영상을 요청한다. 계속 반복한다. 잘 보다가 갑자기 영상이 깨지는 경우 있다. 그러나 끊기는 경우는 적다. 그래서 유튜브를 사용할 때 큰 불만없이 사용하고 있다.

 이러한 방식으로 request되는 건 알겠다. request 서버가 캘리포니아에 있는데, 모든 사람이 request하면 과부화되지 않겠나. 수퍼 울트라 거대 서버를 작동하지 않는 이상 문제가 생길 것이다. 특히 서버 오가는 통로가 막힐 것. 실제 유튜브 관리하는 서버는 한 군데 있더라도 영상을 저장하는 서버는 전세계에 분산되어 있다. 

#### CDN(Content Distributed Network)

![img](https://cdn.hosting.kr/wp-content/uploads/2017/03/How-CDN-works-flow.png)

 지구 상 여러 군데에 동영상 컨텐츠 저장소를 분산시켜 주고, 사용자의 가장 가까운 곳에서 서비스를 제공하도록 하는 것. 테이블 자체는 유튜브 서버에서 가져오더라도 테이블 안의 url은 근처 지역에서 서비스하는 회사일 수도 있다. 

 가장 가까이 있는 주소를 어떻게 아냐. 어느 IP는 지리적으로 어느 위치에 있다는 것을 저장해 놓은 DB가 있다. 이걸 확인해서 ㄱㄱ.