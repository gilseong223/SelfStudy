# Transport Layer

[TOC]

서버 쪽 TCP와 클라이언트 쪽 TCP가 어떻게 생겼는지에 관심있음. 

## 네트워크 시스템

 추상화된 계층으로 이루어져 있다. Application 계층이 가장 위에 존재. 우리가 깐 어플 전부는 어플리케이션 네트워크를 이용하는 것. 편지지에 편지를 넣어서 우체통에 넣는 과정이 Transport layer로 가는 작업이다. 

 우체통에 편지가 들어간 이후에 어떻게 되는지 일반 개발자나 사용자는 모른다. 우리가 프로그래밍할 때 printf 함수 호출만 하지 어떤 일이 일어나는지 알지는 못한다. 이것처럼 점점 layer 아래로 내려갈수록 내부 동작을 자세히 설명하는 단계라고 생각하면 된다. Network layer, Link Layer(케이블, 와이파이, 블루투스 등), physical layer. 

 Transport layer의 전송단위를 세그먼트라고 한다. Application 전송단위는 메시지. 편지지가 편지봉투에 담기는 것이 Application layer -> Transport layer 이다. 편지 봉투에 주소가 있어야 한다. 데이터를 전송하기 위한 부가 정보.  

 Network layer의 전송단위는 패킷. transport layer에서 받은 정보에 헤더 추가. 그 다음 Link layer의 전송단위는 프레임. physical layer에서 라우터를 통해 다른 네트워크에 연결. 이런 과정.

 결국 http request를 보내기 위해 이 과정을 다 거치는 것. 보내기 위해 부가적으로 붙인 오버헤드이다. 그래서 이 부분은 작을 수록 좋다. 편지 볻투엔 최소한의 내용만 적혀 있어야 한다. 그래서 네트워크 설계하는 사람은 이 부분을 최소화 시키려고 노력한다. 필요한 정보만 담도록. 꼭 헤더에 적혀야 하는 것만 적을 수 있게 구조를 만들어 놨다. 정해놓은 필드 이외에 다른 애들을 넣을 수 없도록.  어떤 정보들이 적히느냐가 중요한 이슈. 

 특정 프로토콜의 헤더를 이해한다는 말은 해당 프로토콜이 제공하는 기능이 무엇인지 이해한다는 것이다. 해당 프로토콜의 필드들은 이 프로토콜에서 기능을 수행하기 위해  필요한 정보들이기 때문이다.

![1569392396039](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1569392396039.png)

## Multiplexing

 상대 머신에 도착했을 때, 이게 어느 곳으로 갈지 알려주는 것. 멀티플렉싱은 여러 갈래에서 오는 애를 하나의 통로로 보내는 것. 보내는 쪽에서 일어난다. 디멀티플렉싱은 받는 쪽에서 일어난다. 이를 가능하게 하는 것은 포트번호이다. 

 프로세스와 프로세스간의 커뮤니케이션. 맞는 말이지만 더 자세히 말하자면, 특정 소켓과 특정 소켓 사이의 커뮤니케이션. 한 프로세스가 소켓 하나만 가지고 있는 것은 아니다. 여러 개 만들면 여러 개 생기는 것. 그렇기 때문에 특정 소켓과 특정 소켓 간의 커뮤니케이션이라고 보는 것이 더 정확하다. 

 포트 번호를 가지고 디멀티플렉싱한다. 구글의 웹 서버 프로세스를 예로 들자. 2명의 유저는 구글로 갈 때 포트번호 80으로 갈 것이다. 그러면 막히는 거 아니냐. 그래서 포트번호로만은 디멀티플렉싱이 안된다. 내 소켓과 구글의 소켓은 전 세계에 유니크하다. 이 페어가 어떻게 유니크 하냐. src ip, port, dst ip, port 이 네 가지를 가지고 있어서 유니크 하다.

## Connection-oriented Demux

 Transport layer는 TCP UDP 밖에 없다. 일단 TCP만 생각해보자. tcp 소켓은 분명 지구상 어딘가 존재하는 다른 하나와 쌍이 된다. 이 소켓은  분명 짝이 있다. 무조건 페어하다. 

 UDP는 아무 소켓이나 아무 네트워크에 전달할 수 있다.  UDP 소켓은 소켓간에 연결된 것이 없다. 커넥션이라는 개념이 없고, 어느 누구나 이 소켓으로 줄 수 있다.

 udp 소켓이 여러 개 있는데, 어느 udp 소켓으로 줄 지 판단 해야 함. udp 소켓은 destination port번호만 알면 됨. 

 udp segment에 어떤 헤더가 들어갈까. udp 하는 일이 뭐야. 멀티플렉싱 디멀티플렉싱. 이거 해야하니까 port 번호가 필요하다. length는 헤더를 포함한 UDP 세그먼트의 길이. 그리고 checksum이라는 에러 디텍션 하는거. 에러 나면 버림. 그래서 데이터를 못받는 한이 있어도 에러난 데이터를 받지는 않는다. port번호는 2^16까지 사용할 수 있다. 

 udp 왜 쓰냐. 제공하는 것이 적지만 그만큼 가볍기 때문에.

![udp header](https://notes.shichao.io/tcpv1/figure_10-2.png)

## Principle of Reliable Data Tranfer

 reliable하게 데이터를 전송하기 위해 쓰는 방법은 무엇일까. 실제로는 unreliable 한데 Application layer에게 reliable하게 보이려면 어떻게 해야할까.

### Unreliable한 환경에선 어떤 일이 일어날까

 패킷이 유실되거나 패킷에 에러가 생긴다. 이 두 가지만 잡으면 reliable할 수 있다. 

### 패킷 에러가 발생할 수 있는 환경

 에러를 극복해야함. 에러가 났는지 아닌지 판단. checksum이 필요. 

 친구랑 통화하는데 어쩌다 끊기는 경우가 있다. 상대가 10분 동안 말할 때 나는 가만히 있나? 아니다. 어, 어? 어~ 어! 등의 반응을 한다. 에러 없이 받았다.는 피드백을 주는 것. 

 즉, 피드백이 필요하다. ACK, NAK. 메시지에 checksum을 붙여서 판단해보고 잘 받았으면 ACK, 에러가 나면 NAK. NAK 받으면 재전송.

 피드백이 전송되는 도중 에러가 날 수 있다. NAK인지 ACK인지 알 수 없다. 피드백에도 checksum이 필요하다. 이 피드백이 보낸 그대로인지 아닌지. 에러 있고 negative면 재전송하면 된다. error있고 피드백의 checksum도 에러면? 재전송한다. 그런데 재전송하게 되면 수신자 입장에서는 이게 다음 정보인지 재전송인지 알지 못한다. 그래서 필요한 것이 시퀀스 넘버. 내가 현재 제공하는 것이 이전 데이터인지 다음 데이터인지. 

 점점 해야할 일이 많아진다. 저장공간도 필요해지고. 많아지니까 ACK, NAK을 구분하지 말자. 피드백은 무조건 ACK다. ACK로 Sequnce num 19번이 왔다. 그러면 19번 까지의 sequence는 잘 받았다는 것. 나는 그러면 20번부터 보내면 되는 것이다.

### 패킷 유실이 발생할 수 있는 환경

 전화를 하다 갑자기 안들리면 여보세요 한다. 분명히 둘 중의 한 명은 이야기 해야하는데 침묵이 흐른다. 그러면 긴장감이 생기고 10초 정도 흐르면 여보세요 한다.

 Loss가 가능한 환경에서는 Loss에 대한 대책으로서 타이머를 켜놓고 타이머 시간이 지날동안 피드백을 받지 못하면 재전송. 타이머의 시간은 어떻게 맞춰야 하나. 기준이 없다. 적당히 잘 맞추는 수 밖에.

### Stop-and-wait operation

 메시지를 하나 보내고 피드백을 받기까지 기다려야 함. 아무일도 안하고. 이 시간을 RTT(round trip time?)라고 함. 비효율적이다. Pipelining 적용. 가능한 만큼 쏟아붇고 그거에 대한 피드백 받는 것. 그러면 관리하기 힘듦. 

## TCP

| Application Layer |
| :---------------: |
|        TCP        |
|        IP         |
|       Link        |
|     Physical      |

- Connection Oriented

  : 연결되면 전세계에서 유일한 페어가 된다

### TCP Segment structure

![tcp header에 대한 이미지 검색결과](http://telescript.denayer.wenk.be/~hcr/cn/idoceo/images/tcp_header.gif)

- port : 포트번호가 가질 수 있는 번호는 2^16까지.
- ACK : 1이면 Feedback인 ack의 역할을 한다.
- receive window : 리시버가 받을 수 있는 능력에 맞게 줘야함. 리시버가 얼마나 받을 수 있다는 것을 알려줌. 공간이 작으면 작은대로 sender가 결정함. flow control과 관련있는 비트.

### Sequence Numbers

 TCP에서 sequence number는 바이트 넘버이다. 처음 데이터는 0으로 시작하고 바이트 크기가 100이었다면, 두번째 데이터는 100으로 시작한다. 그리고 크기가 50이라면 다음 데이터는 150으로 시작하게 된다.

### Acknowledgement

 TCP에서 ACK#10의 의미는 0번부터 9번까지 잘 받았다. 이제 10번 기다리고 있다는 의미. Ack는 Cumulative ACK이다. 

![1569477789180](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1569477789180.png)

 보낸 메시지들을 저장할 send buffer가 존재하고, 들어오는 메시지를 저장할 (순서대로 들어오지 않음) receive buffer가 존재한다. 보낼 데이터를 한꺼번에 보내는 것은 아니니까, window size가 정해져있고 각 데이터마다 sequence 번호가 적혀있다. sender와 receiver가 정해져있지 않다. 누구나 sender이면서 receiver이다. 나의 Sequence num은 상대방이 트래킹한 ACK num이고, 상대방의 Seq num은 내가 트래킹한 ACK num 이다.

### TCP Round trip time

 RTT가 10ms인 경우, 타이머는 이것보다 길게 해야한다. 그러나 RTT는 고정된 값이 아니다. 너무 길어도 안되고 너무 짧아도 안된다. 과학에 의해서 타이머를 설정하는 것이 아니라 엔지니어링 측면에서 이렇게 하니까 잘 되더라 정도만 알고있으면 된다. 최적화된 타이머 값은 쓸 수 없다는 것. 

Timeout Interval = EstimatedRTT + "Safety margin"

![1569479774263](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1569479774263.png)

- Sendbase는 window의 가장 앞부분이다.

![1569479918602](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1569479918602.png)

 segment를 받았는데 바로 ack 보내지말고 잠시 기다렸다가 보내라. 왜? 받을때마다 일일이 ack보내는 것보다 조금 기다렸다가 쌓아서 cumulative ack로 보내는 게 낫잖아. 

### Fast Retransmit

 패킷 유실 상황을 타이머로 짐작할 수 있긴 하지만 좀 더 빠르게 알아낼 수 없을까. 어떤 상황을 목격하면 유실이라고 판단할 수 있을까. 

window size가 1000이고 각 sequence #가 100, 200, 300, 400, 500, ... 돼있다. seq 200인 segment가 유실돼서 도착하지 못했다. 그런데 주는 쪽에서는 300을 줬다. 그러면 ack에 그대로 200을 준다. 또 400을 받으면 ack에 200을 준다. 이렇게 3번 같은 ack를 받으면 재전송한다. 정상적인 ack 200과 합쳐서 총 4개의 ack를 받으면 재전송 하는 것이다. 

### 잠깐의 리뷰

 Application Layer에서 tcp 소켓을 통해 내려오면 send buffer와 receive buffer가 생긴다. 우리가 tcp socket을 오픈하는 순간 os가 이러한 자료구조를 만든다. 데이터를 보내려고 할 때, 한 번에 모든 데이터를 보낼 수는 없고, window라는 크기만큼 보낼 수 있다. 보내는 쪽 send buffer의 sequence number는 받는 쪽의 receive buffer에서 관리한다.

 buffer가 왜 필요할까. send buffer는 언제까지 보관할까. 상대방이 받았다고 확신할 때까지 보관한다. 상대방이 확실이 받았는지 보장하지 못하니까 buffer를 둬서 다시 보내기 위해 보관해둔다. receive buffer는 왜 필요할까. 데이터가 날라오는데 순서대로 온 건지 보장할 수 없다. 순서대로 들어오지 않은 애를 기다리기 위해 미리 들어온 애들이 기다린다.  Reliable Data Transfer라는 입장에서 Send buffer는 필수적이다. 상대가 못 받으면 다시 보내야 하니까. 그러나 Receive Buffer는 필수는 아니다. Cumulative Ack니까 못받은건 다시 요청하면 된다. 그래도 reliable은 유지된다. 

![1569803674961](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1569803674961.png)

![1569804137419](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1569804137419.png)

### Flow Control

 상대방의 receive buffer가 얼마나 남았는지만 알면 된다. 상대의 버퍼에 공간이 없으면 보내는 것을 멈추고, 처리속도가 빨라서 계속 공간이 나면 더 빨리 보내주면 된다. 상대의 버퍼에 남아있는 공간만큼 보여주면 된다. 그 정보는 상대의 ack 정보 안에 있기 때문에 알기 쉽다.

 그렇기 때문에 윈도우 사이즈는 항상 고정된 값은 아니다. send buffer에서 window size는 받는 애의 receive buffer에서 남은 공간보다 작게 설정된다.

 receive buffer에 남은 공간이 없어 ack에 receive window 0이라고 보내면, send buffer는 기다린다. 이러한 케이스를 대비해서 센더는 주기적으로 적은 데이터를 보낸다. ack를 받아내기 위해. 

### Nagle Algorithm : Segment를 언제 보내야 할까

 Segment의 최대 크기는 1460byte이다. 이것을 꽉 채워서 보내는게 좋을텐데 어떻게 하면 그럴 수 있을까. 알고리즘은 다음과 같다

- 처음에 데이터가 들어오면 데이터 크기와 상관없이 일단 보낸다.
- 두번째부터는
  - ACK가 도착했을 경우, 쌓인 데이터를 보낸다.
  - Segment 크기만큼 데이터가 쌓일 경우, 데이터를 보낸다.
- 두번째를 계속 반복한다.

### Receiver의 ReceiveWindow

 충분한 공간이 있을 때까지 계속 0으로 ack를 보낸다. 현재 receive buffer에 공간이 없다는 말은 receiver의 데이터 처리 속도가 느리다는 말이다. 적은 양의 데이터를 계속 받아봐야 어차피 시간이 오래 걸린다. 그리고 통신에 데이터보다 헤더가 더 큰 상황이 발생한다. 

### Connection Management

 3-way handshake. 즉 세그먼트가 세 번 교환됨으로써 커넥션이 이루어진다. 2-way로 하면 왜 안될까.  클라이언트가 연결하자고 요청하면 서버가 대답하는 것이 2-way일 것인데, 서버의 대답이 도착한다는 보장이 없다. 

#### 3-way handshake

![three way handshake에 대한 이미지 검색결과](https://learningnetwork.cisco.com/resources/statics/1219267/av440f1_transport_p1_q2_poster.jpg)

 처음에 클라이언트가 SYN bit를 1로해서 보낸다. 서버가 tcp connection할 의향이 있으면 응답을 보낸다. SYNACK SYN bit이 1이면서 ACK를 보낸다. 그러면 다시 클라이언트는 받아서 ACK를 보낸다.

#### termination

![four way handshake에 대한 이미지 검색결과](https://t1.daumcdn.net/cfile/tistory/2336285058D7288E33)

 클라이언트가 더 이상 보낼 것이 없는 경우, FIN bit을 1로 보낸다. 서버는 ack를 보내고 난 후, 서버가 보내야 할 데이터 다 보낸 후에 FIN  bit을 1로 보낸다. 클라이언트도 여기에 대해서 ack를 보낸다. 클라이언트는 보내고 마지막 ack를 보낸 후 바로 끝나는 것이 아니라 아직 연결상태를 기다려야 한다. 마지막 FIN에 대한 ack가 유실될 경우를 대비해서. 