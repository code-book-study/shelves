# chapter2 네트워크

작성자 : 이명욱

## 네트워크의 기초

### 네트워크 토폴로지
- 트리 토폴로지: 노드의 추가 삭제 쉬움, 트래픽 집중시 하위 노드에 영향
- 버스 토플로지: 근거리 통신망(LAN)에 사용, 설치 비용 적고 신뢰성 우수, 노드 추가 삭제 쉬움, *스푸핑 문제
- 스타 토폴로지: 노드 추가 에러 탐지 쉬움, 중앙 노드 발생 -> 전체 네트워크 마비
- 링형 토폴로지: 손실 거의 x, 충돌 가능성 적고 고장 발견 쉽게 찾음, 회선 장애 -> 전체 네트워크 영향
- 메시 토폴로지: 한 장치에 장애 발생 -> 여러 경로 있어 네트워크 계속 사용 가능, 트래픽 분산 처리, 노드 추가 어려움

> *스푸핑: 스위칭 기능 마비 악의적인 도드에 전달되게 됨

### 병목 현상
- 토플로지가 중요한 이유는 병목 현상을 찾을 때 중요한 기준
- 주된 원인
  - 네트워크 대역폭
  - 네트워크 토폴로지
  - 서버 CPU, 메모리 사용량
  - 비효율적인 네트워크 구성

### 네트워크 분류
LAN(개인, 사무실) < MAN(도시) < WAN(세계)



## TCP/IP 4계층 
링크 계층(물, 데), 인터넷 계층(네), 전송 계층(전), 애플리케이션 계층(세, 표, 응)

- 특정 계층이 변경되었을 때 다른 계층이 영향을 받지 않도록 설계
### 1. 애플리케이션 계층
FTP, HTTP, SSH, SMTP, DNS 등 응용 프로그램이 사용되는 프로토콜 계층(실질적 서비스)
> *DNS: 도메인 이름과 IP 주소를 매핑해주는 서버 [Root DNS] -> [.com DNS] -> [.naver DNS] -> [www.DNS]

### 2. 전송 계층
송신자와 수신자를 연결하는 통신 서비스 제공

#### TCP
가상 회선 패킷 교환 방식 - `신뢰성`, `순서대로`
- `3-웨이 핸드셰이크`: SYN(클 isn) -> SYN + ACK(클 isn+1, 서버 isn) -> ACK(서버 isn + 1)
- 4-웨이 핸드셰이크 : FIN(->), ACK(<-) FIN(<-) ACK(->)
  - TIME_WAIT: 지연 패킷 발생 경우 대비, 연결이 닫혔는지 확인
#### UDP
데이터그램 패킷 교환 방식 - `최적의 경로`, `순서 보장 x`

### 3. 인터넷 계층
장치로부터 받은 네트워크 패킷을 IP 주소로 지정된 목적지로 전송하기 위해 사용(IP, ARP, ICMP emd), 비연결형

### 4. 링크 계층
전선, 광섬유, 무선 등으로 실질적으로 데이터를 전달하며 장치 간에 신호를 주고받는 규칙을 정하는 계층

### PDU(Protocol Data Unit)
계층에서 계층으로 데이터가 전달될 때 한 덩어리의 단위
- 애플리케이션: 메시지
- 전송: 세그먼트(TCP), 데이터그램(UDP)
- 인터넷:- 패킷
- 링크: 프레임(데이터 링크 계층), 비트(물리 계층)

## 네트워크 기기
- 애플리케이션: L7 스위치(로드 밸런서)
- 전송: 라우터, L3 스위치
- 인터넷: L2 스위치, 브리지
- 링크: NIC, 리피터, AP

## IP 주소
### ARP
IP 주소로부터 MAC 주소를 구하는 IP와 MAC 주소의 다리 역할을 하는 프로토콜
- ARP: 논리적 주소 -> 물리적 주소
- RARP: 물리적 주소 -> 논리적 주소

### 홉바이홉 통신
IP 주소를 통해 통신하는 과정 = 홉바이홉 통신
- 라우팅 테이블: 목적적지 정보 + 목적지 가기 위한 방법
- 게이트웨이: 서로 다른 통신망, 프로토콜을 사용하는 네트워크 간의 통신 가능하게 하는 컴퓨터 or 소프트웨어

### IP 주소 체계
- IPv4: 32비트(8비트 단위)
- IPv6: 64비트(16비트 단위)

#### 클래스 기반 할당 방식
네트워크 주소 + 호스트 주소

#### DHCP
IP 주소 및 기타 통신 매개변수를 자동으로 할당하기 위한 네트워크 관리 프로토콜

#### NAT
패킷이 라우팅 장치를 통해 전송되는 동안 패킷의 IP 주소 정보를 수정하여 IP 주소를 다른 주소로 매핑


## HTTP

### HTTP/1.0
한 연결당 하나의 요청 처리 *RTT 증가(파일을 가져올때마다 TCP 3-웨이 핸드셰이크)
- 해결 방안: 이미지 스플리팅, 코드 압축, 이미지 Base64 인코딩

> *RTT: 패킷이 목적지에 도달하고 나서 다시 출발지로 돌아오기까지 걸리는 시간이며 패킷 왕복 시간

### HTTP/1.1
한 번 TCP 초기화 이후 `keep-alive` 옵션으로 여러 개의 파일 송수신(1.1부터 기본 옵션)
- 리소스(이미지, 동영상, css, js 파일 등) 개수에 비례해서 대기 시간 길어진다는 단점
- HOL Blocking(Head Of Line Blocking): 같은 큐에 있는 패킷이 첫 번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상
- 무거운 헤더 구조: 쿠키 등 많은 메타데이터, 압축 X

### HTTP/2
HTTP/1.x 보다 지연 시간 줄이고 응답 시간 더 빠르게
- 멀티플렉싱: 여러개의 *스트림 사용하여 송수신
- 헤더 압축: HPACK 압축(허프만 코딩 압축 알고리즘)
- 서버 푸시: 클라이언트 요청 없이 서버가 바로 리소스 푸시

> *스트림: 시간이 지남에 따라 사용할 수 있게 되는 일련의 데이터 요소를 가리키는 데이터 흐름

### HTTPS
SSL/TLS 계층을 넣은 신뢰할 수 있는 HTTP 요청(통신을 암호화)
- 보안 세션: 보안이 시작되고 끝나는 동안 유지되는 *세션
- 인증 메커니즘: CA에서 발급한 인증서를 기반으로
- 암호화 알고리즘: ECDHE or DHE (둘다 디피-헬만 방식)

> *세션: 운영체제가 어떠한 사용자로부터 자신의 자산 이용을 허락하는 일정한 기간

#### HTTPS 구축 방법
- CA에서 구매한 인증키를 기반으로 HTTP 서비스 구축
- 서버 앞단의 HTTPS를 제공하는 로드밸런서 두기
- 서버 앞단에 HTTPS를 제공하는 CDN 두기

> HTTPS 서비스를 하는 사이트가 SEO 순위 높음!(구글 오피셜)<br />
> SEO 관리 -> 캐노니컬 설정, 메타 설정, 페이지 속도 개선, 사이트맵 관리

### HTTP/3
TCP 위에서 돌아가는 HTTP/2와 달리 `QUIC이라는 계층 위에서 돌아가며, TCP 기반이 아닌 UDP 기반`
- 초기 연결 설정 시 지연 시간 감소: 3-웨이 핸드셰이크 과정 거치지 X
- QUIC는 첫 연결 설정에 1-RTT만 소요
- QUIC는 *순방향 오류 수정 메커니즘(Forword Error Correction) 적용

> *FEC: 전송한 패킷이 손실 -> 수신 측에서 에러를 검출하고 수정 + 열악한 네트워크 환경에서도 낮은 패킷 손실률
