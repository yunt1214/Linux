# SSH-Server

- TCP 보안 채널을 형성하여 그 위에서 기타 응용 프로토콜이 안전하게 데이터를 교환하도록 함
- SSL과 비슷하게 비대칭키+대칭키 방식을 사용하여 데이터를 암호화 함

## SSH Key 접속

- 비대칭키 방식을 이용하여 공개키를 접속하려는 서버에 전송 후 개인키, 공개키 인증을 이용하여 접속

### SSH Config 설정

######

    # vim /etc/ssh/sshd_config
    .....
	17행 ssh 서비스 포트 설정
	38행 root 사용자 접속 허용 여부
	43행 공개키 인증 설정
	47행 공개키 저장 위치 및 키 이름
	65행 아이디 / 비밀번호 인증 설정
	(키, 비번 인증 둘 다 설정 시 키 우선)
    .....

## 리눅스 클라이언트

### 1. 키 생성

    $ ssh-keygen

	$ ssh-keygen -t rsa -b 2048 // 키 생성 기본 값은 rsa, 2048bit

### 2. 키 확인

- 계정의 홈 디렉토리에 .ssh 디렉토리 확인
- id_rsa (개인키),  id_rsa.pub (공개키) 
- 파일 내용 확인 가능

### 3. 키 전송

    $ ssh-copy-id dk@192.168.56.5

### 4. 서버에서 키 확인

- 계정의 홈 디렉토리에 .ssh 디렉토리 안에 authorized_keys 확인

## 윈도우 클라이언트

### 1. 키 생성 (접속 애플리케이션마다 다름)

- 푸티젠 실행 -> 제네레이트
- Save Private Key로 개인키 저장

### 2. 공개키 전송

- Puttygen의 공개키를 저장을 해서 사용해도 되지만, 약간의 저장방식이 다르기때문에 수정을 거쳐야 함
- Puttygen 상단에 나오는 공개키 내용을 전체 복사하여 서버의 공개키 파일에 붙여넣기

### 3. Putty 설정
- 좌측 커넥션 -> SSH -> Auth의 오른쪽 젤 하단에 개인키 지정 후 접속
- 공개키의 경우 퍼미션 문제로 인해 접속이 안될 수가 있음

## sshd 접근 제어

### TCP Wrapper를 이용한 접근제어

- /etc/hosts.allow와 /etc/hosts.deny를 이용
- /etc/hosts.allow : 화이트리스트 방식으로 설정된 IP에서만 허용
- /etc/hosts.deny : 블랙리스트 방식으로 설정된 IP에서 접근 차단
- 문법은 hosts.allow, hosts.deny 동일
- [서비스명] : [호스트] 형태로 입력

######

    ex) 특정 IP에서만 SSH 접근 허용 ( 10.100.100.100 )

    # vim /etc/hosts.deny
    .....
    SSHD : ALL
    .....

    # vim /etc/hosts.allow
    .....
    SSHD : 10.100.100.100
    .....

### /etc/ssh/sshd_config

- Allow와 Deny 구문으로 계정 및 그룹 접근 통제
- AllowUsers root dk // root와 dk만 접속 가능
- DenyUsers admin // admin 계정 접속 불가
- AllowGroups wheel // wheel 그룹만 접속 가능
- DenyGroups etc // etc 그룹 접속 불가
- [계정]@[IP] 사용 가능