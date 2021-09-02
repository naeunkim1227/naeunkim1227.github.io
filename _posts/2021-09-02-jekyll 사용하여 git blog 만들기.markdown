---
layout: post
title:  "Welcome to Jekyll!"
date:   2021-09-02 23:39:01 +0900
categories: jekyll update
---
## 사용한 테마
Future Imperfect
: http://jekyllthemes.org/themes/future-imperfect/

에서  JEKYLL YAT THEME 로 변경하였다.

테마다운은 여기 > http://jekyllthemes.org/themes/jekyll-theme-yat/



#### 1. 초기 설정

_config.yml에서 본인의 정보로 수정하면 된다. 
baseurl에 보통 테마제작자의 깃허브 링크가 적혀있는데, 이를 안 지우면 jekyll테마가 적용이 안되므로 주의해야한다.되는 사람도 있겠지만 일단 나는 안 됐다.
이것 때문에 삽질 아주 오래했다...^^



#### 2. CATEGORY 설정

1. category1,2는 모두 내가 원하는 이름으로 변경하였다.(PROJECT,RECORD 등등..)
2. _data 파일 > links. yml에서 원하는 파일명으로 변경, 추가해주면 된다.
3. 글작성은 <b>_post</b> 폴더를 만들면 되는데, 내가 사용한 테마는 미리 작성되있는 것이 있었기 때문에 따로 만들진 않았다.




# 깃허브 블로그 작성법


" # " h1~h6 태그

" --- " 구분선

" ``` " 박스


### post 작성 시 가장 위에 작성해야 할  항목
```
layout: post 
title:  "Lorem"
date:   2017-06-04 00:00
category: category_name
icon: git
keywords: tag1, tag2
image: 1.png
preview: 0