# ownCloud

- 요구사항 : 7.0.7 버전 이상의 PHP

## ownCloud 설치

- 1\. 필요 패키지

        httpd             mariadb-server      php          php-mysqlnd      php-gd
        php-mbstring      php-pecl-zip        php-xml      php-json         php-intl

- 2\. 솔루션 다운로드 (Wget)

        # dnf install -y wget

        # wget https://download.owncloud.com/server/stable/owncloud-10.3.0.zip

- 3\. 솔루션 압축 해제

        # dnf install -y unzip

        # unzip owncloud-10.3.0.zip

- 4\. 솔루션 위치 이동

        # mv owncloud /var/www/html

- 5\. owncloud 디렉토리에 data 디렉토리 생성

        # mkdir /var/www/html/owncloud/data

- 6\. owncloud 디렉토리와 하위 디렉토리 소유권 변경

        # chown -R apache.apache /var/www/html/owncloud // -R : 하위 디렉토리까지 변경해주는 옵션

- 7\. 서비스 시작 및 시작 서비스 등록

        # systemctl restart httpd mariadb // 서비스 재시작

        # systemctl enable httpd mariadb // 서비스 활성화 (부팅 시 자동 실행)

- 8\. owncloud용 데이터베이스 만들기

        # mysql
        .....
        > create schema ocDB; // ocDB 라는 데이터베이스 생성
        > grant all privileges on ocDB.\* to ocadmin@localhost identified by 'Password'; // ocadmin / Password 계정에게 ocDB에 대한 모든 권한 부여
        .....

- 9\. 방화벽 설정 및 확인

        # firewall-cmd --add-port=80/tcp

        # firewall-cmd --add-port=80/tcp --permanent // permanant : 영구적 등록

        # firewall-cmd --list-all

- 10\. 포트 상태 확인

        # netstat -nltp | grep 80

        # netstat -nltp | grep http

- 11\. 접속 확인

        http://[ownCloud 서버의 IP주소]/owncloud

- 12\.SELinux 끄기

        # setenforce 0 // 일시적

        # vim /etc/selinux/config // 영구적
        .....
        7 SELINUX=disabled // enforcing을 disabled로 변경
        .....