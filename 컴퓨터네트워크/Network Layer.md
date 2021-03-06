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

#### Forwarding Table

 Prefix만 본다. 라우터 안의 포워딩 테이블은 네트웍 Prefix로만 이루어져 있다. 네트웍 IP로만 구성되어 있다. 알맞은 엔트리를 찾아서 거기에 적힌 방향으로 보내면 끝. 

#### Subnet

IP 주소는 Network ID, Host ID로 나뉜다. Network id를 Subnet이라고 부르기도 한다. 그래서 Ip는 subnet 파트, host 파트로 나뉜다. Subnet은 어떤 디바이스의 집합니다. 공통된 서브넷 주소를 가지고 있는 디바이스의 집합. 같은 서브넷에 존재하는 디바이스끼리는 라우터를 거칠 필요가 없다. 다른 서브넷에 있는 디바이스에 접근하기 위해서는 라우터를 거쳐야 한다. 서브넷은 물리적으로 라우터로 나뉘어 있다. 라우터는 특수한 디바이스이다. 호스트는 하나의 서브넷에 속해있지만 라우터는 여러 개의 서브넷에 속해있다. 그래서 자신이 속한 서브넷을 포워딩하는 역할을 한다.

![1569997667245](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1569997667245.png)

 여기서 서브넷은 총 6개이다. 라우터의 개수만큼 서브넷을 건너야 한다.

#### NAT(Network Address Translation)

 IP주소공간 고갈에 대한 두려움을 1990년대 초중반에 느끼기 시작했다. 인터넷이 상업화되기 시작하면서. 1996년 IPv6를 디자인했다. 가장 큰 차이점은 IPv6는 주소공간이 128bits 라는 것. 지구상에 존재하는 모래의 개수보다 많고, IoT 막 다 연결해도 남아. 문제없어. 그러나 현재도 IPv4를 쓰고 있다. 왜? 1) IP주소공간이 부족하지 않았는데 엄살부렸거나 2) 부족했는데 다른 방안을 세운 건지. 우리가 가지고 있는 device 개수는 32bit에서 수용할 수 있는 것보다 훨씬 많다. 이미 뛰어 넘었다.

 ![1569998066629](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1569998066629.png)

 inside에 수용할 수 있는 host 개수보다 훨씬 많은 device가 있는 경우. 외부에 있는 구글 서버에 패킷을 보낼 때, 외부적으로 유일한 IP주소로 source 주소를 바꿔치기 한다. 그리고 다시 들어올 때, IP 주소를 NAT가 기억하고 있다가 받아서 가져다 준다. 즉, 내부적으로 unique한 IP주소가 있는데, NAT를 통해 외부적으로 unique한 IP주소로 변환한다. 또 NAT가 IP주소를 가지고 있어서 외부에서 접근하면 연결시켜준다. LTE로 통신을 할 경우, 구글에서 우리의 IP주소를 어떻게 볼까. KT의 데이터 라우터 주소가 찍혀있다. 이렇게해서 IPv4에서 IPv6로 갈아타지 않았다.

![1569998567327](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\1569998567327.png)

 공유기를 가족들이 다 쓰고 있다. 전부 랩탑, 핸드폰 모두 다 IP 주소가 있다. 공유기가 NAT이다. 공유기에 연결된 랜선의 IP는 public unique 주소이다. 랩탑으로 http request해서 네이버에 접근 요청한다. 그러면 NAT translation table에 랩탑의 private IP 주소와 포트를 저장해두고, 글로벌 IP의 주소와 포트를 연결해둔다.

 이러한 과정이 네트워크를 가진 거의 모든 군집에서 사용되고 있다. 우리집 공유기도 NAT가 될 수 있고, KT에서 준 LAN도 사실은 NAT일 수 있다. 이런식으로 사용한다. 1) 장점 : IPv6로 가지 않고 해결했다. IPv6에 가기 위해선 호환성문제가 발생하는데 비용이 발생한다. 네트워크를 위해 라우터를 들러야 하는데, 라우터가 IPv6를 해석할 수 있는 기계여야 한다. 바꾸는 과정에서 돈 많이 든다. 2) 단점1 : NAT 뒤에 호스트가 있다. 우리는 철저히 감춰져 있다. 웹 브라우징 할 수 있지만 웹 서버는 돌리지 못한다. 외부에서 이 호스트를 지칭할 방법이 없다. 웹 서버를 돌렸다. 친구한테 내 주소를 찾아 와라 했을 때, 접근할 수 없다. 감춰져있기 때문에. 테이블에 적히기 위해선 http request 등으로 나가야 한다. 그런데 서버는 오는 걸 기다리는 애이다. 외부에서 접근하게 하고 싶으면 KT에 전화해서 NAT에 올려주세요. 80번 포트에서 돌리고 있다. 외부에서 들어오는 포트가 0000인 경우에는 내 private ip의 80번 포트로 바꿔주세요. IP의 원래 목적을 잊었다. 단점2 : 포트번호는 어떤 소켓을 사용할 것인가로 사용해야 하는데, 어떤 서버인가를 파악하는 것으로 변질되고 있음. 

### DHCP(Dynamic Host configuration Protocol)

 와이파이를 연결하는 순간 자동으로 IP가 세팅되어 있다. ip는 누가 배정해주나. 전체 호스트 기기만큼 ip주소를 제공하는 것이 아니라, 전체 호스트 기기보단 적지만 네트워크 연결에 필요로 하는 개수만큼 배분하는 방식이 있다. 내가 디바이스를 켰을 때, 내가 따로 연결하지 않아도 자동으로 인터넷에 연결해준다.

 와이파이에 연결하기 전에는 랩탑에 ip주소가 없는 상태이다. 랩탑을 켜고 와이파이에 연결하면, DHCP 서버는 24시간 연결되어 있다. 랩탑에서 IP주소를 받아오기 위해 일련의 메시지들이 DHCP 서버와 오간다. 

![dhcp](https://i.imgur.com/LxmbGKq.png)

1) DHCP Discover 

 0.0.0.0 이라는 말은 현재 소스ip가 없다는 의미이다. 255.255.255.255는 브로드캐스트 하겠다는 뜻. 그러면 연결되어있는 모든 호스트가 받는다. 그 중 DHCP가 받는다. 목적지는 모든 호스트인데 포트가 67이다. dhcp 호스트는 68번 포트에서, dhcp 서버는 67번 포트에서 돌아가도록 되어있다. 그러면 나머지 호스트에도 메시지가 가지만 나머지 애들은 67번 포트가 열려있지 않기 때문에 응답하지 않는다. 

2) DHCP Offer

 DHCP도 브로드캐스트해서 클라이언트로 보낸다. 클라이언트는 Transaction id를 확인해서 이 메시지가 나를 위한 것인지 알 수 있다. 

3) DHCP request, DHCP ACK

 네가 준 ip사용하겠다고 서버에게 메시지 보낸다. transaction id + 1해서 보냄. 서버는 여기에 응답하고 이제 클라이언트는 이 ip를 사용한다. 

 DHCP 서버가 다수일 수도 있다. 이런 경우, 클라이언트가 discover로 브로드캐스트 했을 때 모든 서버가 다 받아서 offer를 보낸다. 그러면 클라이언트는 다수의 offer를 받은 것. 그러면 나는 내가 가지고 있는 인터페이스 개수만큼만 ip를 받을 수 있으니까, 1개를 골라야 한다. 그래서 하나의 서베에서 준 ip를 쓰려고 할 때, 여기에 해당하는 DHCP에만 알려주면(ip주소를 지정해서 request를 보내면) 다른 애들은 결과를 받지 못하고 기다리고 있을 것이다. 그래서 브로드캐스트로 보내고 내가 ip를 쓰지 않은 서버는 offer했던 ip주소를 다른 호스트에게 나눠줄 수 있게 생긴다.

Q. DHCP가 커버할 수 있는 범위?
A. 서브넷 개수만큼. 111.111.111.000의 서브넷이라면 2^8만큼 가능하겠지. 그러나 현실적으로는 NAT 쓰고 있기 때문에 충분히 사용할 수 있음

#### IP주소만 받으면 끝일까? No

 나와 바로 연결되어 있는 Gateway router의 ip주소는 알아야 한다. DNS 서버의 ip주소, 정확히는 Local Name Server의 ip 주소(우리가 사용할 DNS name server는 우리의 Local network 안에 있다)를 알아야 웹 브라우징 할 것이다. 그러면 DNS에서 받은 ip주소를 dest addr에 넣는다. 이렇한 정보들이 세팅이 되어야 웹 브라우징이 가능할 것이다.

 DHCP는 게이트웨이에서 동작한다. 게이트웨이는 NAT도 한다. 또한 밖으로 나가는 gateway router 역할도 한다. 여기서 잡다한 일 많이 함. 집에서 랩탑 키면 공유기가 NAT, DHCP 연결을 다 해준다. 공유기가 게이트웨이 이다.

### IP Fragmentation, Reassemble

 커다란 IP 패킷이 있으면, 헤더는 하나이다. 이것이 라우터가 수용할 수 있는 범위를 넘어서면 패킷을 쪼갠다. 이것이 Fragmentation. 분해가 됐을 때 다시 조립할 수 있도록 부가적인 정보를 담는 공간이 헤더의 length, Identification, flag, offset 이다.

### example

![example](https://i.imgur.com/GiL0Gv0.png)

 4000byte datagram, MTU(Maximum Tansmission Unit) = 1500bytes. 인 경우 3개의 패킷으로 나뉠 수 있을 것이다. id = 패킷의 id이다. 그래서 3개로 나뉜 패킷의 id는 모두 같다. 내 다음에 패킷이 더 있는 경우 flag는 1, 없을 경우 0으로 둔다. length가 4000이니까 헤더 크기 20이고 데이터 크기 3980이다. 그러면 데이터를 3등분 해야하므로, 첫번째 패킷의 헤더 20과 데이터 1480 오프셋 0, 두번째 패킷의 헤더 20과 데이터 1480 오프셋 1480이 될 것이다. 그런데 그림을 보면 185가 되어있다. 1480 / 8 한 것이다. 일단 목적은 헤더의 크기를 줄이기 위한 것. 뒤의 3비트를 줄이겠다는 의미. 세번째 패킷은 헤더 20에 남은 데이터 크기 1020이 된다. 그리고 뒤에 데이터 없으니까 필드 0으로 해주고, length 1040으로 넣어준다.

 이러한 정보를 토대로 dest host에서 reassemble한다. flag, offset 둘다 0인 애는 따로 조립할 필요가 없는 애고, 그렇지 않은 애는 기다려서 모든 ip 패킷을 다 받은 후 재조립한다. 중간에 하나라도 유실되면 재조립하지 못한다. 그러면 tcp timer가 다되면 다시 전송해서 받을 수 있다. 네트워크는 best effort를 하고, tcp에서 reliable하게 조정해준다. 

#### ICMP(Internet Control Message Protocol)

 internet protocol이 하는 일은 패킷이 도착지까지 도착하도록 하는 것. ip packet은 사용자가 보내고자 하는 데이터를 담아서 보내주는 봉투이다. 라우터 간 네트워크 해야할 경우도 있다. 유저 메시지가 아니라 제어(Contorl) 메시지도 있다. 네트웍 상의 문제가 생겼을 때 리포트 하는데 이때 ICMP를 사용한다.

#### IPv6

 address는 128bit. 기존 32bit에 비해 훨씬 많아졌다. 거의 무한대.... ;; IPv6 패킷을 만들어서 보내면 안간다 ;; 패킷을 만들 순 있지만 보낼때 router가 이해하지 못한다. 라우터는 IPv4의 헤더만 이해할 수 있다. 모든 라우터를 바꿔야 하는데 쉽지 않은 문제이다. 한국어를 사용하고 있는데 내일부터 우리나라 불어쓴다 하면 쉽지 않은것처럼. IPv4에 동작하는 머신과 IPv6가 공존하는 머신이 필요하다. 터널링이 필요한데, 이것은 IPv6 패킷을 IPv4 헤더를 붙인다. 

### Routing

 네트워크 레이어에서 중요한 디바이스. 라우터의 가장 중요한 기능은 1) 포워딩 2) 라우팅 이다. 포워딩은 단순한데, 처음에 Forwarding Table은 누가 적은 것이냐. Routing Algorithm으로 가능하다. 목적지까지 최소 비용으로 갈 수 있는 경로.

#### Link State(OSPF)

 다익스트라 알고리즘. 왜 Link state냐. 링크스테이트 알고리즘의 기본 가정 : 네트워크 상의 모든 라우터는 네트워크가 어떻게 구성되어 있는지 전체 그림을 안다. 각자 브로드캐스트해서 각자의 정보, 코스트 등을 알고 여기에서 알고리즘을 시작하는 것이 Link state 알고리즘이다. 

 [다익스트라][https://ratsgo.github.io/data%20structure&algorithm/2017/11/26/dijkstra/]로 테이블을 구하고 난 후, Forwading table에 넣으면 된다.

#### Distance Vector(RIP)

 모든 노드는 자기 이웃과만 정보를 교환한다. 링크 스테이트처럼 브로드 캐스트가 아니다. 그리고 [벨만포드 알고리즘][https://ratsgo.github.io/data%20structure&algorithm/2017/11/27/bellmanford/].

#### Link Cost

 링크 코스트는 시간이 지남에 따라 변할 것이다. 변하는 상황을 보자. Distance Vertor의 알고리즘 동작에 따르면 어떤 일이 일어날까. 링크 코스트가 바꼈으니까, 당장 자신의 링크 테이블을 바꿔야 한다. 링크 상태가 좋아지면 몇 라운드만 거치면 바로 값을 적용할 수 있는데, 상태가 안좋아질 경우, 연산할 것이 많아진다. 

Countin Infinity, poison reverse

### Routing in the Internet

#### 현실적인 문제

 전세계 라우터가 1억개라고 치자. 1억개를 돌리면 말이 안됨. 억지로 돌려서 수렴시켜도 링크 코스트 체인지가 생긴다. 어떻게 해야 하나. Scale 문제가 나오면 무조건 Hieirarchical하게 분류해야 한다.

 학교 네트워크 내에서는 호스트끼리 알고리즘 쓸 것이다. 그러면 우리학교랑 외부 네트워크는 어떻게 하냐. 그건 상위 네트워크가 알아서 알고리즘 쓴다. 

#### BGP(Border Gateway Protocol)

 네트워크를 넘나드는 경로에 대한 라우팅 프로토콜. 학부 수준에서 다루기엔 방대한 내용. 간단하게 설명.

##### AS(Autonomous System)

 하나의 네트워크. 소유자가 독립적으로 가지고 있는 네트워크. 내부 네트워크. 각 as에는 게이트웨이 라우터가 존재한다. as 외부로 나갈 수 있는 라우터. BGP는 inter AS 끼리의 라우팅 방식을 정하는 것.

 게이트웨이를 빠져나와서 AS들을 거쳐서 목적지까지 갈 것이다. 이것을 걱정하는 것이 BGP이다. as는 다같이 as라고 하지만 위상이 같지는 않다. 시립대 네트워크도 as고, 외대도 as, kt도 as이다. as들은 트래픽을 주고 받는다. 이 사이에 케이블이 있다. 시립대에서 외부로 데이터 보내고 싶으면 뭐가 연결되어 있을 것이다. 시립대가 KT에 돈을 주고 링크를 가져온다. as 사이에서 갑을 관계는 어떻게 정해지나. KT는 시립대에 접근할 필요는 없는데 시립대는 KT의 서비스가 필요하다. KT는 또 AT&T에게 사용료를 낸다. SKT와 KT가 만나면 갑을 관계는 아니다. 동등한 입장이니까 돈을 주고받지 않고 트래픽만 왔다갔다 한다. 이러한 관계는 영원하진 않다. 구글 10년 전엔 저 밑에 있었는데 점점 올라가고 있다. 직접 AT&T와 계약을 할 수도 있고. 

 AS 사이의 경로는 비즈니스 관계에 의해 결정되는 것이고, Shortest Path 이런 알고리즘은 아니다. 아무런 이득없이 전달해주는 경우는 발생하지 않는다. 돈 되는 길로 간다.