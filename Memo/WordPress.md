# WordPress

- 요구사항 : 7.4 버전 이상의 PHP, 10.3 버전 이상의 MariaDB

## wordpress 설치

- 1\. 서비스에 필요한 패키지

######

        httpd, mariadb-server, php, php-mysqlnd

- 2\. 패키지 버전 확인

######

        # dnf info mariadb-server php
        .....
        Name       : mariadb
        Version    : 10.3.28
        Name       : php
        Version    : 7.2.24 // 버전 업그레이드 필요
        .....

- 3\. 패키지 설치

######

        # dnf install -y epel-release // 버전 업그레이드를 위해 EPEL 설치

        # dnf module enable -y php:7.4 // 7.4 버전 설정

        # dnf info php
        .....
        Name       : php
        Version    : 7.4.30 // 버전 업그레이드 확인
        .....

        # dnf install -y httpd, mariadb-server, php, php-mysqlnd

- 4\. 서비스 시작

######

        # systemctl restart httpd mariadb // 서비스 재시작

        # systemctl enable httpd mariadb // 서비스 활성화 (부팅 시 자동 실행)

- 5\. 서비스 확인

######

        # systemctl status httpd mariadb

- 6\. 솔루션 다운로드 및 설정

######

        # dnf install -y wget tar

        # wget https://ko.wordpress.org/latest-ko_KR.tar.gz

        # tar xfz latest-ko_KR.tar.gz

        # mv wordpress /var/www/html

        # cp /var/www/html/wordpress/wp-config-sample.php wp-config.php

- 7\. 워드프레스용 데이터베이스 만들기

######

        # mysql
        .....
        > create database wpDB; // wpDB 라는 데이터베이스 생성
        > grant all privileges on wpDB.* to wpadmin@localhost identified by 'Password'; // wpadmin / Password 계정에게 wpDB에 대한 모든 권한 부여
        .....

- 8\. 워드프레스 설정

######

        # vim /var/www/html/wordpress/wp-config.php // 데이터베이스 정보 수정
        .....
        23 define( 'DB_NAME', 'wpDB' );
        24
        25 /** Database username */
        26 define( 'DB_USER', 'wpadmin' );
        27
        28 /** Database password */
        29 define( 'DB_PASSWORD', 'Password' );
        30
        31 /** Database hostname */
        32 define( 'DB_HOST', 'localhost' );
        .....

        # chown -R apache.apache /var/www/html/wordpress/ // 소유권 변경. -R : 하위 디렉토리까지 변경해주는 옵션

        # vim /etc/httpd/conf/httpd.conf
        .....
        122 DocumentRoot "/var/www/html/wordpress" // 경로 수정
        .....

        # systemctl restart httpd // 서비스 재시작

- 9\. 방화벽 설정

######

        # firewall-cmd --add-service=http (--add-port=80/tcp)

        # firewall-cmd --add-service=http --permanent (--add-port=80/tcp --permanent) // permanant : 영구적 등록

- 10\. 방화벽 설정 확인

######

        # firewall-cmd --list-all

- 11\. 포트 상태 확인

######

        # dnf install -y net-tools

        # netstat -nltp | grep 80

        # netstat -nltp | grep http

- 12\. 접속 확인

######

http://[WordPress 서버의 IP주소]/

## SELinux 끄기

- SELinux 보안 정책에 의해 서비스가 막힐 수 있기 때문에 사용하지 않는다.

######

      # setenforce 0 // 일시적

      # vim /etc/selinux/config // 영구적
        .....
        7 SELINUX=disabled // enforcing을 disabled로 변경
        .....
