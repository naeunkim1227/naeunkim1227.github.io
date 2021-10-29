---
layout: post
title: github에 maven project 만들기, eclipse 내려받기
subtitle: github
categories: github
tags: [java,github,maven]
published: false
---

github에서 공용으로 사용할 maven project를 생성하고 module을 여러개 생성했는데,
이 프로젝트를 github에 올리고 다시 내려받으니 mavenproject와 module로 받아지지 않고, 이것 저것 오류가 나타났다.
그래서 차례대로 기록해 놓으려 한다.

**구글에 검색해도 mavenproject 깃허브 관련글이 많이 없어 나중에 저처럼 삽질할 분들이 있을까 싶어 적어 놓는 글이며, 더 좋은 방법이 있을 수도 있습니다.**




### 1.github에서 repository 생성하기
    
깃허브에서 레포지토리를 먼저 생성한 뒤 주소를 복사한다.
그 다음 이클립스 탭에서

1. window > show view > other에서
git검색후 git repositoy를 클릭한다.

혹은 
    
 2. 이클립스 상단 우측의 open perspective에서 git을 클릭해도 된다.
    
나는 2번 방법으로 진행했다.   


![gitrepo](https://user-images.githubusercontent.com/83413364/132783513-d17d34ac-a564-4659-a6e7-6716818eb5e2.png)


### 2.이클립스에서 git repository 생성하기

위의 git repository에서 clone a git repositoy를 선택한다.
깃레포지토리가 하나도 없다면 화면에 바로 떠있을 것이고 아무것도 없다면 우측 상단에 초록색 화살표 그림이 있는 이미지 버튼이 clone a git repositoy이다. 


![clone](https://user-images.githubusercontent.com/83413364/132783806-7af053ed-13b0-44a6-9810-dc3eba850449.png)

URI에 깃허브레포지토리를 복사한 주소를 복붙하면 된다. 
USer와 password에 본인의 아이디와 비번을 넣는데, 깃허브가 21.8월 중순이후로 토큰 방식으로 바뀌었다고 해서...! 비밀번호로 입력하면 실행이 안 될 수도 있다. 그러니 토큰을 생성 받아서, 토큰을 넣어준다.


그럼 이클립스로 깃허브 레포지토리가 만들어 진다...

![clone22](https://user-images.githubusercontent.com/83413364/132784089-16463094-cd85-48e2-9c1b-efc86c3307ae.png)

### 3. project 생성하기

위의 레포지토리 우클릭 >  import project 

![import](https://user-images.githubusercontent.com/83413364/132785069-76f0440d-623a-41f2-9780-4bfa76e10ff0.png)

finish 클릭한다. 나는 이미 import한 project라 오류가 떴다. 이제 java ee로 이동하면 프로젝트가 추가 되있을 것이다. 


### 4. maven project로 변경하기

![configure](https://user-images.githubusercontent.com/83413364/132785308-c34ba840-4f8f-4571-9ff2-a407aa395d8f.png)


프로젝트 우클릭해서 configure > convert to maven project 클릭하여 메이븐 프로젝트로 변경해준다!

### 정리 및 후기..

이클립스에서 먼저 메이븐 프로젝트를 생성하고, 이를 올린다음 다시 내려받았을때, 메이븐 프로젝트로 받아지지 않는 오류가 발생했다. > 깃허브에서 먼저 레포지토리를 받아와 프로젝트를 생성하고 메이븐 프로젝트로 변경하는 방식으로 변경했다.

참고로 메이븐 프로젝트까지는 정상적으로 만들어 졌는데 모듈이 모듈형식으로 받아와지지 않는 경우도 있었다. 
나는 팀과 공유를 하는 상황이었어서, 내가 팀원들 모듈을 다 만들고 깃허브에 커밋을 했는데 이를 다른 팀원들이 clone했을때 , 모듈로 만들어지지 않았다. 그래서 메이븐 프로젝트까지만 생성하고 각자 모듈을 생성하고 커밋, 풀을 한 결과 모두 모듈형식으로 만들어졌다.

혹시나 특정 팀원것이 모듈로 안 생성 되었다?  이럼...깃 레포지 토리, 프로젝트 모두 삭제하고 다시 임포트하면 제대로 땡겨와졌다.

이상..매우 삽질하여 성공한 메이븐 프로젝트 생성 끝...


