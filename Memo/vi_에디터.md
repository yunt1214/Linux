# vi 에디터

- 파일을 편집하기 위한 프로그램

######

        G	    파일의 제일 마지막 행으로 이동
        gg	    파일의 제일 첫 행으로 이동
        i	    입력모드 진입
        #         주석 처리 (실제 설정에는 적용 안 됨. 설명문)
        :se nu    행번호 표시
        :숫자     '숫자'행으로 이동
        :w	    저장
        :wq	    저장하고 나가기
        :q	    그냥 나가기
        :q!	    강제로 나가기 (변경 내용이 있을 경우 그냥 나가기 안됨)
        -----------------------------------------------------------
        yy	    행 복사 (숫자 yy)
        p	    붙여넣기
        dd 	    행 삭제 (숫자 dd)
        u	    되돌리기

## 파일 내용 확인

### cat

- 파일의 내용을 화면에 출력
- 표준 입력으로 받은 값을 표준 출력으로 이어주는 명령
- 리다이렉션 기호와 함께 파일을 생성하거나 여러 개의 텍스트 파일을 합치는 기능을 수행

######

        # cat [파일 이름]

### more, less

- 파일의 내용을 한 페이지씩 출력
- less가 more를 발전시켜 나온 명령어
- 스페이스, 페이지 업 다운, 엔터, 방향키로 이동
- /[패턴]으로 패턴 검색 가능, 다음 단어는 n
- 나갈 때는 q

######

        # more 또는 less [파일]

### head

- 파일의 처음부터 10행 출력
- -n : 처음부터 n행 출력

######

        # head [파일]

### tail

- 파일의 끝부터 10행 출력
- -n : 마지막부터 n행 출력
- -f : 파일에 추가되는 내용을 계속 출력

######

        # tail [파일]
