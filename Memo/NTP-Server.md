# NTP-Server

- Network Time Protocol
- 정확한 시간 및 날짜 정보를 제공
- 네트워크의 컴퓨터 시스템 간 시간 동기화 서비스 제공
- 시스템 간 시간의 불일치로 인해 발생 가능한 문제를 해결하기 위해 사용함
- UDP 123

## Stratum

- Stratum 0 : 원자시계, GPS 시계등 시간 정보를 최초로 생성하는 장비
- Stratum 1 : ST-0 장비에 직접 연결된 대형 시간 서버

## NTP 서버 설치

### 1. 패키지 설치

######

    # yum install -y ntp

### 2. 설정

######

    # vim /etc/ntp.conf
	21 ~ 24 : 시간을 동기화 할 서버 지정

### 3. 서비스 재시작

### 4. 시간 동기화 확인

######

    # ntpq -p (또는 ntpq -pn)

######

- ntpq -p 또는 ntpq -pn으로 확인
- remote : 동기화 (시도)하고 있는 NTP서버
- refid : 리모트 서버가 동기화하고 있는 NTP 서버
- st : stratum
- t : 시간을 받는 방식 ( unicast, mutilcast, broadcast )
- when : NTP서버로부터 데이터를 수신한 후 경과 시간.
- poll : NTP 서버로 동기화 요청 주기
- reach : 최근 8번 poll 요청에 대한 응답 여부. 1, 3, 7, 17, 37, 177, 377 값으로 표시될 경우 정상
- delay : 네트워크 지연 시간
- offset : NTP 서버와 로컬의 시간 차이
- 서버 앞 기호 표시
- \* : 현재 동기화하고 있는 서버
- \+ : 접속은 가능하지만 동기화는 안됨
- \- : 접속은 가능하지만 동기화리스트에서 제외
-	기호가 없을 경우 접속 불가능한 서버

## 네트워크 장비에서의 설정

### Client Mode

    R1#sh clock // 시간 보기

    R1(config)#clock timezone KST 9 // 타임존 맞추기

    R1(config)#ntp server 192.168.100.123 // NTP 서버 지정

    R1#clock set 10:40:00 19 feb 2024 // 수동으로 시간 맞추기

### Server Mode

    R1(config)#ntp master [st값]

### NTP 확인

    R2#sh ntp associations

    R2#sh ntp status

# 시간 관련

## date

- 현재 서버에 세팅되어있는 날짜 및 시간을 출력
- 수동으로 시간 조정 가능

######

    # date mmddHHMMyyyy

## timedatectl

- 날짜 및 시간, 타임존 출력

######

    # timedatectl 

    # timedatectl list-timezones

    # timedatectl list-timezones | grep -i seoul

    # timedatectl set-timezone Asia/Seoul

## rdate

   - 인터넷의 타임서버와 시간 동기화 (일회성)

######

    # rdate -s time.bora.net