---
layout: post
title: Linux 서버에서 git,maven,jenkins 설치
subtitle: git,maven,jenkins 설치
categories: Linux
tags: [Linux, 설치]
---



**post 2021-09-28**

설치할 목록은 다음과 같다. douzone 디렉터리 안에 모아 둘 것이기 때문에
**mkdir douzone** 으로 디렉토리를 생성한다.

```
설치 목록

/usr/local/douzone

— java

— tomcat

— git

- maven

— mariadb

— node

— python3

```

** .tar.gz 형식의 파일로 설치했다. **

대부분의 파일은 방식이 비슷하다.
1. 링크를 복사해 wget으로 파일 다운로드
2. tar xvfz 로 압축 풀기
3. mv 로 douzone 폴더에 옮기기
4. ln -s 로 링크작업 - 좀 더 쉽게 명령어를 입력하여 파일을 실행 할 수 있도록 링크를 연결한다. 
5. /etc/profile 에서 PATH 수정 


## 1. git 설치
버전 2.9.5
기본적으로 git이 설치 되있기 때문에 업데이트 개념이다.
작업위치 /root

1. 의존성 라이브러리 설치하기
    ```
    $ yum install curl-devel
    $ yum install expat-devel
    $ yum install gettext-devel
    $ yum install openssl-devel
    $ yum install zlib-devel
    $ yum install perl-devel
	$ yum install asciidoc
	$ yum install xmlto
    ```
2. 다운로드
    ```
     $ wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.9.5.tar.gz
    ```

3. 압축풀기

    ```
    $ tar xvfz git-2.9.5.tar.gz
    ```

4. configure 
    ```
    $ ./configure --prefix=/usr/local/douzone/git
    ```

5. 빌드
    ```
    $ make all doc html info
    ```

6. 설치
    ```
    $ make install install-doc install-html install-info
    ```

7. PATH 설정
    ```
    $ vi /etc/profile
    ```
    다음 내용을 추가한다.

    ```
    # git
    PATH=$PATH:/usr/local/douzone/git/bin
    ```

8. git 환경 설정
    ```
    $ git config --global user.name "사용자 닉네임"
    $ git config --global user.email "사용자 이메일"
    ```

9. git 사용하기

    디렉토리를 먼저 생성한 후에 git init 으로 git 전용 공간을 생성해준다.

    ```
    $ mkdir centos-practices
    $ cd centos-practices
    $ git init
    $ git remote add origin [생성한 레포지토리 주소]
    $ vi READ.md
    ```
    
    READ.md 파일에 아무 내용 입력하기
    
    ```
    $ git add -A
    $ git commit -m "first commit"
    $ git brunch -M main
    $ git push -u origin main
    ```
10. git 레포지토리에서 확인하기

## 2. maven 설치

작업 디렉토리는 /root

1. 다운로드

    ```
    wget 
    ```

2. 압축풀기

3. 설치

4. PATH 설정



## 3. jenkins 설치