# Network Layer

[TOC]

 인터넷을 이해하기 위해 계층적으로 생각한다. 지난 시간까지 트랜스포트 레이어를 말했다. 트랜스포트 레이어는 양 끝단에 있는 두 호스트간의 메시지 교환을 이야기했다. 이제는 두 호스트 사이에 어떤 연결이 있는지 아는 것. 클라이언트와 서버 사이에 라우터로 이루어진 네트워크가 존재하고 있다. 우체통에 편지를 넣었을 때 어떤 경로를 통해서 도착 우체통으로 도착하는지.

 라우터에서 패킷을 받았을 때, 어느 방향으로 전달할지 정하는 것. forwarding table을 참조해서 알맞은 방향으로 패킷을 전달하는 것이 forwarding이고, 라우터는 forwarding table을 채우는 역할을 한다. 테이블엔 몇번 패킷부터 어디까지 몇 번 라우터로 들어간다는 정보가 적혀있다.

 IP protocol은 가장 중요한 프로토콜 중 하나이기 때문에 이 기능을 이해할 필요가 있다. 일단 Network layer에서는 tcp에서 받은 세그먼트에 헤더를 추가한다. 라우터들은 tcp 헤더를 볼 필요도 없고 봐도 이해 못한다.

![1569889360588](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1569889360588.png)

 destination address가 첫 번째 example이라면 0번 인터페이스로 간다. 두번째 예제라면 1번도 되고 2번도 될 것 같은데, 사실은 가장 길게 매칭이 되는 것에 들어간다. Longest prefix matching. 

## IP: Internet Protocol

 IP는 reliable을 보장하지 않는다. tcp는 포트만 찾았다면 IP는 전세계에서 이 집이 어딘지 확인하는 것이다. 

![ip header에 대한 이미지 검색결과](http://telescript.denayer.wenk.be/~hcr/cn/idoceo/images/ip_header.gif)

- protocol : 데이터에 든게 tcp인지, udp인지
- src, dest address : 가장 중요.

인터넷에 패킷들을 보면 40byte짜리 IP 패킷이 많다. 정체가 뭐야. 영화 다운받을 때를 생각해봐라. http request를 보냈다. response가 쭉 올 것이다. 어플리케이션 레이어 입장에선 한 번이지만, 수많은 tcp ack가 오갈것이다. 바로 이것.

### IP Address(IPv4)

- unique 32-bit number
- Identifies an interface(on a host, on a router ...)

 IP 주소라는 것은 호스트의 네트워크 인터페이스를 지칭하는 것이다. 지금 노트북은 와이파이 인터페이스 하나의 IP주소를 가지고 있다. 핸드폰은 LTE 인터페이스, 와이파이 인터페이스 두 개 있다.

 IP주소를 여러개 가진 device가 있다. 라우터. IP주소는 호스트를 지칭하는 것이 아니라 특정 인터페이스를 지칭하는 것이다. 

#### IP 주소를 랜덤으로 정하는 경우

 라우터 안에 들어있는 forwarding table은 어느 방향으로 패킷을 보내는지 정해놓은 테이블이다. 랜덤으로 IP주소를 정하는 경우 호스트 개수만큼 forwarding table의 컬럼이 주어질 것이다.

#### Hierarchical Addressing : IP Prefixes

32bit 중 앞 부분은 네트웍 구조. 우리가 쓰고 있는 와이파이 IP의 앞부분은 똑같다. 우리 주소의 앞부분이 같으니까. 12.34.158.0/24 는 앞에서부터 24번째까지 prefix다 라고 쓴 것이다. 

 이럴 경우, 새로운 호스트가 등장할 경우에 라우터의 포워딩 테이블을 추가하거나 변경할 필요가 없다. 대신 호스트 IP의 앞부분을 규칙에 맞춰야 함.

 그러면 내가 수용할 수 있는 개수가 몇 개일까. 24번째까지 prefix라면 256개 밖에 못쓰는 것인가. 200개 이상 허용할 수 있는 network가 되려면 prefix가 작아야 한다.  2000개의 호스트를 받으려면 21번째까지만 prefix로 만들어야 한다. 21개가 network를 위한 ip이고, 남은 11개는 호스트를 위한 IP이다. 