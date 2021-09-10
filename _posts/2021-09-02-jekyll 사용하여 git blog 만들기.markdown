---
layout: post
title:  "jekyll 사용하여 github blog 만들기"
date:   2021-09-02 23:39:01 +0900
categories: gitblog
tags: [gitblog,jekyll]
---
## 사용한 테마
Future Imperfect
: http://jekyllthemes.org/themes/future-imperfect/


21.09.02
JEKYLL YAT THEME로 변경하였다.
테마다운은 여기 > http://jekyllthemes.org/themes/jekyll-theme-yat/

왜냐.. 카테고리나 태그 별로 분리하고 싶었기 때문...그냥 원래 사용한 블로그에 드롭다운 메뉴 추가하고, 나누려고 했으나..하다보니 귀찮아졌다.
무튼 jekyll테마를 적용한 gitblog 만드는 법은 아래와 같다.


## 준비

1. ruby gem
2. git 
3. Ccode
4. 본인의 github
5. jekyll


## 1. git repository 만들기

![210902-repo](https://user-images.githubusercontent.com/83413364/132782267-3a0bd0e4-358e-4357-9f70-1c5e7deefc76.png)

레포지토리 이름은 
아이디.github.io로 작성했다.

naeunkim1227.github.io

## 2. git clone 하기


![210902-git](https://user-images.githubusercontent.com/83413364/132782319-aca964e5-51ec-45e8-ac88-8f6f8883b399.png)

위의 주소를 복사한다. 그 다음 GIT 터미널 창에서 

```
git clone 주소 
```


그럼 repository 폴더가 생성된다.
git 터미널에서 계속 해도 상관은 없지만 나는 ccode에서 작업을 진행한다.

## 3. jekyll 설치


```
gem install jekyll bundler
```
로컬 터미널에서 jekyll을 설치한다. 
이 부분만 로컬에서 진행하고 나머지는 ccode터미널에서 진행!
gem...은 ruby를 설치해야 작성이 가능하므로 설치가 안 돼있다면 설치하자...(아마도...)


```
C:\Users\naeun\Desktop\blog> cd naeunkim1227.github.io
jekyll new ./
```

ccode 터미널에서 블로그 폴더로 이동해준다.
그 다음 위 문구로 jekyll을 설치하고
주의점은 폴더안에 아무것도 없어야 한다. 
테스트 해보기 위해 html파일을 추가했었다가 오류구문이 떴었음


```
bundle install 
```
 그 다음 위 문구를 작성한다. 


 ```
bundle add webrick

bundle exec jekyll serve
 ```

로컬서버에서 테마가 적용이 됐는지 확인해 보려면 위 문구를 작성 후 뜨는 로컬 서버로 적용해 보면 된다. 

## 4. jekyll theme 적용

theme가 있는 github를 fork해서 적용해도 되지만 나는 zip파일로 다운 받았다. 
안에 있는 모든 파일을 복사해서 블로그 폴더 안에 다 넣는다. 
```
bundle
```
그 다음 터미널에서 bundle을 작성해서 테마를 설치한다. 

변덕이 심해 테마를 여러번 바꿔봐서...팁이 있다면...
css가 적용이 되지 않을때가 있다. 


이유는 3가지였다.

* 깃허브페이지에 테마가 적용되는데 생각보다 오랜 시간이 걸린다. 좀 많이 기다려야할때가..있다.
* _config.yml에 있는 baseurl: ""  ,url: "" 에 본인의 깃허브 링크를 넣어주면 된다. 혹은 그냥 비워져 있어야한다. 나는 블로그테마 제작자 깃허브링크가 써져있어서 적용이 안 됐다.
* ruby를 설치해야한다. window같은 경우 package가 있어서 그것만 다운받으면 된다. 


## 5. git에 파일 추가하기

```
git add .
git commit -m "적을 메세지"
git push origin master
```

위의 문구를 작성해주자. 그 다음 블로그 레포지토리에서 settings로 이동 > pages 탭으로 이동한다.


![210902-branch](https://user-images.githubusercontent.com/83413364/132782350-314ce875-58ef-46e4-94e4-67dc1aa2f549.png)

그럼 위와 같이 되어 있을텐데, branch가 none으로 설정돼 있을 것이다. 클릭해서 master로 변경해준다.
나는 push할때 master로 해서 master가 있음. 
main으로 해도 됨..


좀 기다리면 적용이 되어있을 것!


# 깃허브 블로그 작성법


" # " h1~h6 태그

" --- " 구분선

" ``` " 박스
