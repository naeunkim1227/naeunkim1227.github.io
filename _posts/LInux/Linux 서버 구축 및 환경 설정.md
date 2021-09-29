---
layout: post
title: Linux 서버 구축 후 환경설정, java,tomcat 설치
subtitle: Linux환경 java,tomcat 설치
categories: java
tags: [java,개념]
---


# Linux 서버 구축 및 환경설정

사용한 툴
xshell, virtual box, tomcat , eclipse(잠깐 사용)

**매우 초반의 리눅스 환경설정은, 생략하고 xshell연동과정, java와 tomcat 설치를 정리하였다.**


 ## 1.  centos 7 기본작업
- mount -t iso9660 /dev /sr0 /mnt /cdrompw 로 마운트 후 umout /dev/cdrom 으로 마운트 해제 작업하기
    - 마운트 : 디바이스를 사용하기 위해 하드웨어와 디렉토리를 연결하는 작업
- 계정 만들기
    
    작업위치 /root (pwd 로 항상 작업 위치를 보고, cd 명령어로 /root에서 작업하자)
    
    useradd -g wheel -d /home/webmaster webmaster
    
    -g 뒤는 그룹명, -d 뒤에는 계정 홈 디렉터리
    
    passwd webmaster
    
    비밀번호 설정하기
    
- yum repolist 로 패키지 확인 후  yum update 입력하여 패키지 업데이트
- 설치할 패키지 yum -y  install ~~
    - cronie
    - rdate
    - gcc
    - make
    - wget
    - gcc-c++
    - cmake
    - net-tools
    - bind-utils
    - psmisc
        
    
## 2.  Xshell 과 연동하기
    
   ![xshell](https://user-images.githubusercontent.com/83413364/134882601-bad80995-a068-4c8c-b006-63addf48b285.png)

xshell 의 좌측 상단에 +파일 버튼이 있다. 이 것을 클릭하면 
위의 창이 뜨는데, 이름에는 자신이 넣고자 하는 이름
호스트에 포트 번호를 넣으면 된다. 나는 자신의 포트 번호인 127.0.0.1을 넣었다


연결이 완료 되면
ssh 127.0.0.1 혹은 open 명령어로 접속이 가능하다.
생성한 계정, 비밀번호로 접속하면 된다.
    


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



## 01. 자바 설치하기


1. java8 se 리눅스 버전으로 설치한다.
2. 연결하기
*(새로운 터미널 열어서 작업해야함
,ssh 입력 안한 터미널)
```
 $ sftp webmaster@127.0.0.1
```
비밀번호 입력 후,

3. linux 서버에 파일 넣기

원래 터미널에서 작업
```

$cd /usr/local/douzone
$ put C:\Users...파일있는 경로\jdk-8u291-linux-x64.tar.gz

$ exit
```

4. 압축 푼 후 설치 되었는지 확인한다.
```
$ tar xvfz jdk-8u291-linux-x64.tar.gz

$ ls -la
```

압축이 풀리지 않은 파일이 ~~.tar.gz
풀린 파일은 tar.gz이 없는 파일이름과 같다.

5. douzone 폴더로 이동 시키기

```
$ mv jdk1.8.0_291 /usr/local/douzone/java
```

6. 링크 연결

```
$ ln -s jdk1.8.0_291/ java
```
위와 같이 링크 연결 후, java를 입력하면 jdk1.8.0_291를 연결 해주는 것이다.


7. java 명령어만 입력해도 파일이 실행 될 수 있도록 profile을 편집한다.

```
$ vi /etc/profile

```
입력후 i를 눌러 수정, 가장 아래에 

```
#java


export PATH=$PATH:/usr/local/douzone/java/bin
export CLASSPATH=.:/usr/local/douzone/java/lib/tools.jar

```

겹치는 주소가 많으니 변수를 선언하여 다음과 같이 축약할 수도 있다.

```
#java

export JAVA_HOME=/usr/local/douzone/java

export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib/tools.jar


```
esc + :wq 로 저장하고 나온다.

8. 소스 명령어로 업데이트 하기 

```
$ source /etc/profile

```


9. 컴파일 하기

```
$ java
$ javac
```

에디터를 열어서 class 를 생성해준다.

```
$ vi Helloworld.java

```

```
public class Helloworld{
    public static void main(String[] args){
        System.out.println("Hello!");
    }

}
```

```
$ javac Helloworld.java
$ java Helloworld

```

입력한 내용인 Hello가 나오면 성공적인 설치 완료!


## 02. tomcat 설치하기

1. 작업위치는 /root
wget 으로 바로 다운 받는다.

```
$ wget https://mirror.navercorp.com/apache/tomcat/tomcat-8/v8.5.65/bin/apache-tomcat-8.5.65.tar.gz
```

2. 압축 풀기
```
$ tar xvfz apache-tomcat-8.5.65.tar.gz
```

3. 이동하기

```
$  mv apache-tomcat-8.5.65 /usr/local/douzone/tomcat8.5
```

4. 링크 연결
```
$ ln -s /usr/local/douzone/tomcat8.5 /usr/local/douzone2021/tomcat
```

5. 실행 및 실행여부 확인하기

``
$ /usr/local/douzone/tomcat/bin/catalina.sh start

$ ps -ef | grep java 
``


6. 포트 포워딩

    Virtual Box에서 설정 > 네트워크 > 고급 > 포트 포워딩

![tomcat](https://user-images.githubusercontent.com/83413364/135248423-4bb32c4f-1b18-43ff-aba3-decbe2cff7d6.png)

다음과 같이 설정한다.

7. 포트 확인

```
$ vi /usr/local/douzone/tomcat/conf/server.xml
```

![화면 캡처 2021-09-29 190907](https://user-images.githubusercontent.com/83413364/135248966-87c9629c-f3ea-4483-91ab-13a3cb470822.png)

다음과 같은 화면에서 /8080 검색후 

바로 아래

```
Connector port="8088" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <!-- A "Connector" using the shared thread pool-->

```

이 부분의 포트를 8088로 수정해준다.


8. 실행

```
 $ 0/usr/local/douzone/tomcat/bin/catalina.sh start
 $ ps -ef | grep tomcat
 $ ps -ef | grep java
```
실행 되고 있는 지 확인한다.

9. 브라우저로 접근하기
   http://127.0.0.1:8088 로 접속하여 톰캣 페이지가 뜨면 연결 완료!


10. 중지시키기

```
$ /usr/local/douzone/tomcat/bin/catalina.sh stop
```


11. 서비스 등록하기

```
   $ /usr/lib/systemd/system/tomcat.service
```
파일을 생성한 후 다음 내용을 넣는다.

```
[unit]
Description=tomcat8
After=network.target syslog.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/local/douzone/java
User=root
Group=root

ExecStart=/usr/local/douzone/tomcat/bin/startup.sh
ExecStop=/usr/local/douzone/tomcat/bin/shutdown.sh

UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target

```

12. tomcat 서비스 실행/중지/재실행

```
$ systemctl start tomcat
$ systemctl stop tomcat
$ systemctl restart tomcat
```

13. tomcat manager 설정

1) tomcat-users.xml 설정

```
$ vi /usr/local/douzone/tomcat/conf/tomcat-users.xml
```

<tomcat-users> 안에 다음 내용 추가하기

```
<tomcat-users>
  . . .
  . . .
  <role rolename="manager"/>
  <role rolename="manager-gui" />
  <role rolename="manager-script" />
  <role rolename="manager-jmx" />
  <role rolename="manager-status" />
  <role rolename="admin"/>
  <user username="admin" password="manager" roles="admin,manager,manager-gui, manager-script, manager-jmx, manager-status"/>

</tomcat-users>

```

2)context.xml 수정

```
$ vi /usr/local/douzone/tomcat/webapps/manager/META-INF/context.xml
```

원래의 아래코드는 주석처리한다.
```
<!--
  <CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor"
                   sameSiteCookies="strict" />
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
-->
```


```
<Context antiResourceLocking="false" privileged="true" docBase="${catalina.home}/webapps/manager">
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="^.*$" />
</Context>
```

14. tomcat 재시작

```

$ systemctl stop tomcat
$ ps -ef | grep tomcat
$ systemctl start tomcat
```


## 03. tomcat 에 war파일 올려 배포하기


1. http://127.0.0.1:8088/ 로 접속하여 tomcat 페이지를 띄운다. 우측 상단의 Manager App 버튼 클릭

![화면 캡처 2021-09-29 194233](https://user-images.githubusercontent.com/83413364/135253738-6fff825a-18fb-46b3-a5ef-a851a1f5117a.png)


war 파일 선택하여, 배치한다.

2. 확인

```
 $ cd /usr/local/douzone/tomcat/webapps
 $ ls -la
```

위치로 이동하면 내가 올린 helloweb war 파일이 올라와 있는지 확인한다. 

주소창에서 http://127.0.0.1:8088/helloweb/index.jsp 를 입력하면 index 페이지가 표시된다.

