---
layout: post
title: Linux 서버에서 git,maven,mariadb,jenkins 설치
subtitle: git,maven,mariadb,jenkins 설치
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



## 3. mariadb 설치

1. 라이브러리 설치

    작업 디렉토리는 /root

    ```
    $ yum install -y gcc
    $ yum install -y gcc-c++
    $ yum install -y libtermcap-devel
    $ yum install -y gdbm-devel
    $ yum install -y zlib*
    $ yum install -y libxml*
    $ yum install -y freetype*
    $ yum install -y libpng*
    $ yum install -y flex
    $ yum install -y gmp
    $ yum install -y ncurses-devel
    $ yum install -y cmake.x86_64
    $ yum install -y libaio
    ```

2. iconv 설치 및 압축 해제

    ```
    $ wget https://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.16.tar.gz

    $ tar xvfz libiconv-1.16.tar.gz

    $ cd libiconv-1.16 
    $ ./configure --prefix=/usr/local

    $ make 
    $ make install
    ```

3. mariaDB 설치

    버전은 10.1.48 이다.

    ```
    $ wget https://downloads.mariadb.org/interstitial/mariadb-10.1.48/source/mariadb-10.1.48.tar.gz/from/https%3A//archive.mariadb.org/

    $ mv index.html  mariadb-10.1.48.tar.gz

    tar xvfz mariadb-10.1.48.tar.gz
    ```

    index.html 로 받아지기 때문에 tar.gz로 변경이 필요하다. 


4. 빌드 환경 설정

    ```
    $ cd mariadb-10.1.48

    $ cmake -DCMAKE_INSTALL_PREFIX=/usr/local/douzone/mariadb -DMYSQL_USER=mysql -DMYSQL_TCP_PORT=3306 -DMYSQL_DATADIR=/usr/local/douzone/mariadb/data -DMYSQL_UNIX_ADDR=/usr/local/douzone/mariadb/tmp/mariadb.sock -DINSTALL_SYSCONFDIR=/usr/local/douzone/mariadb/etc -DINSTALL_SYSCONF2DIR=/usr/local/douzone/mariadb/etc/my.cnf.d -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS=all -DWITH_ARIA_STORAGE_ENGINE=1 -DWITH_XTRADB_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_FEDERATEDX_STORAGE_ENGINE=1 -DWITH_PERFSCHEMA_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DWITH_SSL=bundled -DWITH_ZLIB=system

    $ make
    $ make install
    ```


5. 계정 생성

    ```
    $ groupadd mysql
    $ useradd -M -g mysql mysql
    ```

6. /mariadb 소유자 변경

    ```
    $ chown -R mysql:mysql /usr/local/douzone/mariadb
    ```

7. 설정 파일 위치 변경

    ```
    cp -R /usr/local/douzone/mariadb/etc/my.cnf.d /etc
    ```


8. 기본 데이터베이스 생성

    ```
    $ /usr/local/douzone/mariadb/scripts/mysql_install_db --user=mysql --basedir=/usr/local/douzone/mariadb --defaults-file=/usr/local/douzone/mariadb/etc/my.cnf --datadir=/usr/local/douzone/mariadb/data
    ```


9. 서버 구동

    ```
    $ /usr/local/douzone/mariadb/bin/mysqld_safe &
    ```

10. root 패스워드 설정

    ```
    # /usr/local/douzone/mariadb/bin/mysqladmin -u root password
    ```

11. 데이터 베이스 접속 테스트

    ```
    # /usr/local/douzone/mariadb/bin/mysql -u root -p
    ```

    (root에 한해서는 -u 를 넣지 않아도 된다)

12. PATH 설정
    ```
    vi /etc/profile

    아래 내용 추가
    # mysql
    export PATH=$PATH:/usr/local/douzone/mariadb/bin
    ```


12. 서비스 등록/시작/중지
    ```
    # cp /usr/local/douzone/mariadb/support-files/mysql.server /etc/init.d/mariadb
    # chkconfig mariadb on (centos7이상: systemctl enable mariadb)
    # systemctl start mariadb
    ```

## 4. jenkins 설치