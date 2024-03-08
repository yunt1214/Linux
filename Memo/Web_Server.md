# Web Server

- 서버 중 가장 많이 활용되는 분야
- 오랫동안 안정적으로 많이 사용했던 패키지는 Apache
- LAMP (Linux + Apache + PHP + MySQL)

## 구축하기

- 1\. 서비스에 필요한 패키지

		httpd, mariadb-server, php, php-mysqlnd

- 2\. 패키지 버전 확인

		# yum info httpd mariadb-server php php-mysqlnd

- 3\. 패키지 설치

		# yum install -y httpd mariadb-server php php-mysqlnd

- 4\. 설정 및 확인

		# vim /etc/httpd/conf/httpd.conf
		.....
  		119 DocumentRoot "/var/www/html" // html 파일 경로
		164 DirectoryIndex index.html // html 파일 이름
		.....

- 5\. 서비스 시작

		# systemctl restart httpd mariadb // 서비스 재시작

		# systemctl enable httpd mariadb // 서비스 활성화 (부팅 시 자동 실행)

- 6\. 서비스 확인

		# systemctl status httpd mariadb

- 7\. 방화벽 설정

		# firewall-cmd --add-port=80/tcp (-service=http)

		# firewall-cmd --add-port=80/tcp --permanent (-service=http --permanent) // permanant : 영구적 등록

- 8\. 방화벽 설정 확인

		# firewall-cmd --list-all

- 9\. 포트 상태 확인

		# yum install -y net-tools

		# netstat -nltp | grep 80

		# netstat -nltp | grep http

- 10\. 접속 확인

		http://[웹 서버의 IP주소]/

## 간단한 index 만들기

- http://[웹 서버의 IP주소]/ 에서 확인

		# vim /var/www/html/index.html // 기본으로 설정된 경로와 파일명. httpd.conf에서 변경 가능
		.....
		<h1> Hello, World! </h1>
		.....

## PHP 적용 확인

- http://[웹 서버의 IP 주소]/[PHP 파일 이름].php 에서 확인

		# vim [PHP 파일 이름].php
		.....
		<?php phpinfo(); ?>
		.....

## SELinux 끄기

- SELinux 보안 정책에 의해 서비스가 막힐 수 있기 때문에 사용하지 않는다.

		# setenforce 0 // 일시적

		# vim /etc/selinux/config // 영구적
	 	.....
	 	7 SELINUX=disabled // enforcing을 disabled로 변경
	 	.....
