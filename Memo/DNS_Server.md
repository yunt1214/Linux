# DNS-Server

- Domain Name System Server
- Domain 이름을 IP로 변환해주는 서버
- 컴퓨터 이름 + IP를 hosts 파일에 기록

## 도메인 구조

### Root Level Domain ( . )

- 도메인 구조에서 최상위를 차지하는 도메인
- 일반적으로 브라우저에 입력하는 경우는 생략
- 도메인 설정 시 반드시 표기
- 전 세계에 Root DNS 서버는 13개가 존재함

### Top Level Domain

- ccTLD : .kr, .fr, .sp 등등 국가코드 최상위 도메인
- gTLD : .com, .net, .org 등의 일반 최상위 도메인
- IANA에 의해 관리

### Second Level Domain

- 회사나 조직, 기관 등의 목적에 부합되는 단어 및 숫자를 이용해 생성

### Third Level Domain

- 각 목적에 따라 필요한 경우 생성된 도메인
- 서브 도메인이라고도 함

######

    ex) www.dk.com.

    - 제일 뒤 .이 루트레벨 도메인
    - .com이 탑 레벨 도메인 (gTLD)
    - dk가 세컨드레벨 도메인 (회사나 기관 등을 나타내는 단어)
    - www가 서드레벨 도메인 (dk에서 필요에 의해 만든 도메인)
    - dk.com은 도메인 부분, www는 호스트 부분
    - dk.com은 메인 도메인, www는 서브 도메인

### FQDN (Full Qualified Domain Name)

- 호스트의 이름과 도메인의 이름을 포함한 전체 도메인 이름
- 호스트와 도메인을 함께 명시하여 전체 경로를 모두 표기하는 것

## DNS 서버의 종류

### 캐시 네임 서버

- 사용자들의 질의를 받아 DNS 정보를 조회하여 응답해주는 네임서버

### 권한 네임 서버

- 도메인 존(zone) 데이터를 갖고 있어, 응답하는 네임 서버
- 도메인을 등록할 때 이 서버의 IP를 등록하고 자체 도메인에 대한 정보를 관리

## 리눅스의 DNS 관련 설정 파일

### /etc/hosts

- 자체 설정 forward 파일

### /etc/resolv.conf

- 네임 서버 주소 지정 파일
- 직접 지정은 일회성 (재부팅 시 초기화)
- 네트워크 설정 파일에서 DNS 서버 주소를 지정해야 함

## DNS 서버 구축

### 로컬 DNS 서버

- 1\. 서비스에 필요한 패키지

######

    bind, bind-utils

- 2\. 패키지 설치

######

    # yum install -y bind bind-utils

- 3\. 설정 및 확인

######

    # vim /etc/named.conf
    ......
    13 listen-on port 53 { any; }; // listen 할 네트워크 대역 지정
    21 allow-query { any; }; // query를 허용할 네트워크 대역 지정
    .....

    # named-checkconf /etc/named.conf // named.conf 파일 문법 체크

    # named-checkzone dk.com /var/named/dk.com.db // dk.com.db 파일 문법 체크

- 4\. 서비스 시작 및 확인

######

    # systemctl restart named

    # systemctl enable named // 서비스 활성화 (부팅 시 자동 실행)

    # systemctl status named

- 5\. 방화벽 설정 및 확인

######

    # firewall-cmd --add-port={53/udp,53/tcp}

    # firewall-cmd --add-port={53/udp,53/tcp} --permanent // permanant : 영구적 등록

    # firewall-cmd --list-all

### dig를 통해 IP를 받아오는 경로를 추적

    # dig @192.168.56.10 www.daum.net A +trace

## SELinux 끄기

- SELinux 보안 정책에 의해 서비스가 막힐 수 있기 때문에 사용하지 않는다.

######

    # setenforce 0 // 일시적

    # vim /etc/selinux/config // 영구적
    .....
    7 SELINUX=disabled // enforcing을 disabled로 변경
    .....

## 마스터 DNS 서버

- 회사나 단체에서 제공하는 서비스 or 호스트를 IP로 매핑시켜주는 서버
- 각자 도메인을 생성 (ex. dk.com)
- 정방향 DNS - 도메인을 IP로 매핑

### 마스터 DNS 서버 구축

- 1\. 도메인 설정 (zone 설정)

######

    # vim /etc/named.conf
    .....
    zone "dk.com" IN { // 도메인 이름
        type master;
        file "dk.com.db"; // 데이터베이스 파일명
    };
    .....

- 2\. 존 데이터베이스 파일 설정

######

    # cd /var/named/

    # cp named.empty dk.com.db

    # chown root.named dk.com.db // 소유권 변경

    # vim dk.com.db
    .....
    $TTL 3H
    @       IN SOA dk.com. admin.dk.com. (
                                            20240301 ; serial
                                            1D       ; refresh
                                            1H       ; retry
                                            1W       ; expire
                                            3H )     ; minimum
            NS      ns.dk.com.
            A       192.168.56.10

    ns      A       192.168.56.10
    www     A       192.168.56.5 // Web Server 주소. IP주소 대신 www.dk.com으로 접속 가능
    .....

    $TTL : DNS Cache를 저장하는 기간
    @    : 도메인 네임을 지정
    IN   : 클래스이름, 일반적으로 Internet
    SOA(Start of Authority) : 리소스 레코드 유형을 지정
    serial  : 일련번호. 존 파일 갱신 여부를 표시
    refresh : 갱신 주기
    retry   : 갱신 실패 시 재시도 기간
    expire  : 재시도 계속 실패 시 만료시간
    minimum : 존 파일 TTL값

    A     : 도메인 / 호스트에 대한 IP 주소를 정의
    CNAME : 별칭
    MX    : 메일서버 정의
    NS    : 도메인서버 정의
    PTR IP : 주소에 대응하는 도메인 / 호스트를 정의
