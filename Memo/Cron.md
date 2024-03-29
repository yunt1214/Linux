# Cron

- 어떤 일을 주기적으로 실행해야 할 때 사용
- '정해진 시간'에 반복적으로 작업을 실행

## 주기 설정

######

    *	        *	        *	        *	        *
    분(0-59)	시(0-23)	일(1-31)	월(1-12)	주(0-6)
    
    30          1           *           *           * // 매일 새벽 1시 30분마다

    0           18          10          *           * // 매월 10일 오후 6시마다
    
    30          3           15          1           * // 1월 15일 새벽 3시 30분마다
    
    30          9           *           *           3 // 매주 수요일 9시 30분마다

## 시간 옵션

### *

- 전체 시간

### -

- 숫자 범위
- 분 필드에 1-5 : 1분, 2분, 3분, 4분, 5분을 의미

### ,

- 나열된 수를 의미
- 분 필드에 1,3,5 : 1분, 3분, 5분을 의미

### /

- 특정 단위를 건너뛸 때
- 분 필드에 */5	: 5분마다를 의미

## Crontab

- 사용자가 주기적인 작업을 등록하기 위해 사용
- vi 편집기처럼 사용

### 실행

    # crontab [옵션]
               -l : 크론탭 내용 출력
               -e : 크론탭 작성 및 수정
               -r : 크론탭 삭제
               -u [user] : [user]의 크론탭 - root만 가능

### Crontab 사용제한

- /etc/cron.allow와 /etc/cron.deny로 제한 가능
- 두 파일이 모두 있을 수도 있고, 하나만 있을 수도 있음
- 두 파일 모두 없는 경우는 root만 cron 사용
- 둘 다 있는 경우 allow가 적용

# at

- 지정한 시간에 원하는 명령이나 작업을 실행
- 일회성 작업 예약

######

    at [옵션] [시간]
        -l : 명령 목록 출력
        -r 번호 : [번호] 명령 삭제

    # yum install -y at

    # systemctl restart atd

## 1. 시간 지정 및 명령어 입력

    # at 10:00 tomorrow

    # at 10:00 feb 15

    # at 10:00 + 3days

    # at now + 1hours

    # at 16:00

    # at 4:00pm

## 2. 입력 종료

    Ctrl + d

## 3. 예약 확인

- /var/spool/at : root만 확인 가능

######

    # at -l 또는 atq // 예약 시간 및 작업 번호 확인

## 4. 작업 삭제

    # at -r  [작업 번호]

    # atrm  [작업 번호]