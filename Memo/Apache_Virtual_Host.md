# Apache Virtual Host

- Web Server + DNS Server + Win 7
- WIN 7 의 DNS IP 설정은 192.168.56.10
- DNS DB에서 www, blog, cloud의 IP설정은 192.168.56.5
- WIN 7에서 각각 접속하여 확인
- DNS 캐시 삭제 : Win 7 cmd에서 ipconfig /flushdns 입력

## 가상호스트 (VirtualHost)

- 하나의 아파치 서버가 여러 개의 웹 사이트를 서비스할 때 각각의 사이트를 구분해서 접속하도록 구성하는 것
- 어떤 방식으로 사이트를 구분하냐에 따라 이름 기반, 포트 기반, IP 기반 가상호스트로 나뉨
- 어떤 것으로 접속하냐에 따라 DocumentRoot를 변경함으로써 각 서비스에 맞는 사이트를 열어줄 수 있다.
- 가상호스트 설정 중 밑의 설정이 열리지 못할 경우 제일 위의 설정이 적용되어 페이지가 열린다.

######

    # vim /etc/httpd/conf/httpd.conf // VirtualHost 및 DocumentRoot 설정 확인
    .....
    include /etc/httpd/conf.d // include 구문을 활용 맨 아래에 작성
    .....

    # vim /etc/httpd/conf.d/virtualhost.conf // /etc/httpd/conf.d 안에 파일 생성

### 이름 기반 가상호스트

- 접속하려는 호스트 이름에 따라 DocumentRoot가 변경

1. 접속 확인을 위한 DocumentRoot 및 Index 만들기

2. 접속 설정 (VirtualHost 설정)

######

    <VirtualHost 172.16.31.5>
    ServerName "www.dk.com"
    DocumentRoot "/var/www/html"
    </VirtualHost>

    <VirtualHost 172.16.31.5>
    ServerName "blog.dk.com"
    DocumentRoot "/var/www/html2"
    </VirtualHost>

    <VirtualHost 172.16.31.5>
    ServerName "cloud.dk.com"
    DocumentRoot "/var/www/html3"
    </VirtualHost>

### 포트 기반 가상호스트

- 서버에 접속하는 포트를 기반으로 사이트를 구분
  ex) http://www.dk.com http://www.dk.com:8080

######

    <VirtualHost 172.16.31.5:80>
    ServerName "www.dk.com"
    DocumentRoot "/var/www/html"
    </VirtualHost>

    <VirtualHost 172.16.31.5:8080>
    ServerName "www.dk.com"
    DocumentRoot "/var/www/html2"
    </VirtualHost>

### IP 기반 가상호스트

- 웹 서버에 접속하는 IP를 기반으로 사이트를 구분
- 웹 서버에 여러 개의 IP를 부여 (네트워크카드 추가 혹은 추가 IP부여)

######

    <VirtualHost 172.16.31.5>
    ServerName "www.dk.com"
    DocumentRoot "/var/www/html"
    </VirtualHost>

    <VirtualHost 172.16.31.6>
    ServerName "blog.dk.com"
    DocumentRoot "/var/www/html2"
    </VirtualHost>