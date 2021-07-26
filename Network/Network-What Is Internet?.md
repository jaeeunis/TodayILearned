# 컴퓨터 네트워크 

## 1. What is internet ?

#### 1-1 host (=end system) 

호스트는 인터넷의 모든 사용자 application을 가리키며 최근에는 컴퓨터 이외의 스마트폰, 테블릿 pc 등 많은 기기들도 포함한다.

#### 1-2 packet switches

라우터는 이름 그대로 네트워크와 네트워크 간의 경로를 설정하고 가장 빠른 길로 트래픽을 이끌어주는 네트워크 장비이다. 

#### 1-3 communication link

고전적인 구리선에서 광케이블까지 다양한 매체를 통해 데이터를 전송해준다. 이 각각의 링크들은 다양한 전송률로 데이터를 전송해주며, 전송률은 초당 비트 수를 나타내는 bip(bit per second) 단위로 표현한다.

`RFC(Request for comments)` : 인터넷 표준안 (대학원생들이 만들었는데, 어떠한 프로토콜에 대해서 의견을 요청했기 때문에 이름이 이러하다..) <br>
`IETF (Internet Engineering Task Force)` :  인터넷 표준안을 만드는 집단

<br>

## 2. A closer look at network structure

### Access networks and physical media

- [x] 대역폭 Bandwidth (bits per second)
- [x] 공유하는 또는 독점적인 Shared or Dedicated

#### 2-1 Acess net : home network

#### digital subscriber line (DSL)

전화회사(skt, kt 등등)에서 제공하는 Access network 를 DSL이라고 한다. 일반적으로 공유하지 않는 dedicated link이고, 전화회사 DSLAN으로 전달하고, DSLAN이 일반적으로 전화 또는 인터넷인지 나눠서 전달한다. 일반적으로 인터넷에서 자료를 다운받는 경우가 많기 때문에 downstream transmission rate가 크다. 

#### cable network 

케이블 회사에서 제공하는 Access network 를 cable modem이라고 한다. 일반적으로 공유하는 shared link이고, DSL과 똑같이 CMTS로 전달한다. 여러개의 cable headend가 있고 fiber 링크로 연결한다. fiber, coax(모든 가정들이 공유하는 방식)의 조합으로 이루어져 있기 때문에 **HFC**라고 한다. downstream transmission rate가 크지만 shared 이기 때문에 많은 패킷을 차지할 수 있다는 것을 보장할 수 없다.

<br>

#### 2-2 Enterprise access network

#### Ethernet switch  

한 건물 또는 한층의 이더넷 스위치를 통해서 여러 이더넷 스위치들이 회사 전체를 대표하는 라우터에 연결한다. 라우터는 dedicated 라인을 이용해서 ISP(Internet Service Protocol) 에 직접 연결한다. 전화회사 또는 케이블회사가 아닌 직접 ISP를 설치한다.

<br>

#### 2-3  Wireless access network (shared and wireless)

#### wireless LANs

무선랜은 무선 신호 전달 방식을 이용하여 두 대 이상의 장치를 연결하는 기술이다. 이를 이용해 사용자는 근거리 지역에서 이동하면서도 지속적으로 네트워크에 접근할 수 있다. 주로 건물내에서 사용한다.

#### wide-area wireless access

넓은 지리적 거리/장소를 넘나들며 네트워크 할 수 있다. 또한 대표적으로 셀룰러 네트워크라고 불리는 LTE, 5G, 3G 가 있다.



<hr>

#### 참고

<a href="http://www.kocw.net/home/search/kemView.do?kemId=1046412">이화여자대학교 이미정교수님 컴퓨터 네트워크 강의</a> <br>
Computer Networking: A Top-Down Approach Featuring the Internet, 6th Ed., James F. Kurose and Keith W. Ross, Addison Wesley