---
title: Github 블로그 생성
date: 2020-02-26 10:42:44
tags: blog begineer
---

### 기술블로그 Start
----
2020-02-26 3번째 회사의 퇴사를 앞두고, 보안사항이 아닌 개발관련 정리내용을 옮기기로 결심.    
Confluence의 개인 공간에 작성해 두었던 기술관련 정리내용들을 옮겨보려 했다.

### hexo를 선택하게 된 이유
----
github 블로그를 생성하는 개발자에게 인기가 많은 프레임워크는 아래 3종이 있다.  
- jekyll
  - 가장 많은 사용자를 보유하고 있다.
    - 한글 사용법, 다양한 테마 지원 등의 강점이 있다.
  - ruby 기반의 프레임워크이다.
    - 이부분이 나한테는 단점으로 다가왔다.
- Hexo
  - nodejs 기반의 프레임워크이다.
    - node-express를 사용해본 사람이라면, ejs 문법도 그렇게 어렵지 않다.
    - kubernetes, helm, gitlab을 사용해본 경험으로 YAML configuration 문법이 좀 더 쉽게 다가왔다.
  - 기본 테마, 깔끔한 템플릿은 많이 있다.
  - git으로 포스트 버전관리가 안된다.
    - 별도 포스트 버전관리를 위한 저장소를 지정하고
    - 배포를 지원하는 hexo-deployer-git node package를 사용하여 포스팅 하기로 결정했다.
- Hugo
  - go lang 기반으로 만들어졌으며, 빠르다는 강점이 있다.
    - 필자는 jekyll과 hexo만 사용해봤으며, 좀 더 쉽게 configuration 할 수 있는 hexo를 선택하게 되었다.


### clean-blog theme 를 선택하게 된 이유
----
크게 다른이유는 없다..
cresumerjang님 블로그를 참고하여, 진행하다보니  
comment, discussion 하는 library(disqus)를 사용하기 위해 선택하게 되었다.

### HEXO 블로그 입문 참고 - 첫 블로그 포스팅.
----
[cresumerjang님 블로그 :: hexo 블로그 입문 가이드](https://cresumerjang.github.io/2019/01/17/hexo-start-manual/)

