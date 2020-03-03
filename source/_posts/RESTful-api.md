---
title: RESTful API
date: 2020-03-03 18:12:44
tags: [api, restful, rest]
---
## 정의
REST : Representational State Transfer의 약자  
자원(Resource)을 이름(자원의 표현(represent))으로 구분하여, 해당 자원의 상태(정보)를 주고받는 모든 것을 의미한다.  
HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해 해당 자원에 대한 CRUD(Create, Read, Update, Delete) Operation을 적용하는 것을 의미한다.  

## 장/단점
- 장점
HTTP 프로토콜의 인프라를 그대로 사용하므로, REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.  
HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해준다.  
HTTP 표준 프로토콜을 따르는 모든 플랫폼에서 사용 가능하다.  
Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.  
REST API 메시지가 의도하는 바를 명확하게 나타내므로, 의도하는 바를 쉽게 파악할 수 있다.(e.g.,DELETE - 삭제, GET - 조회)  
여러가지 서비스 디자인에서 생길 수 있는 문제를 최소화 한다.  
서버와 클라이언트의 역할을 명확하게 분리한다.  
- 단점
표준이 존재하지 않는다.(사용하는 주체에 따라, POST를 update, PATCH를 update 로 쓰는 경우가 있더라..)  
사용할 수 있는 HTTP Method가 4가지 밖에 없다.(보안상의 이슈로 사용할 수 있는 HTTP Method에 제약을 두더라, 확실치는 않다.)  
브라우저를 통해 테스트 할 일이 많은 서비스라면, 쉽게 고칠 수 있는 URL 보다 Header 값이 더 어렵게 느껴진다. (GET 방식의 query string이 좀 더 편하게 다가올 수 있다..)  
구형 브라우저에서 제대로 지원하지 못하는 부분이 존재한다.(PUT, DELETE를 사용하지 못하는 점 등.)  

## RESTful API 설계 기본 규칙
- 용어
  - document : 객체 인스턴스나 데이터베이스의 레코드와 유사한 개념.
  - collection : 서버에서 관리하는 directory 리소스(폴더)
  - store : 클라이언트에서 관리하는 리소스 저장소.

- URI는 자원을 표현해야 한다.
  - resource는 동사보다는 명사를, 대문자보다는 소문자를 사용한다.
  - resource의 도큐먼트 이름으로는 단수명사를 사용해야 한다.
  - resource의 컬렉션 이름으로는 복수명사를 사용해야 한다.
  - resource의 스토어 이름으로는 복수명사를 사용해야 한다.

- 자원에 대한 행위는 HTTP Method(GET, PUT, POST, DELETE 등)로 표현한다.
  - URI에 HTTP Method가 들어가면 안된다.
  - URI에 행위에 대한 동사표현이 들어가면 안된다.
  - 경로 부분 중, 변하는 부분은 유일한 값으로 대체한다. (자원을 구분할 수 있는 Key)

## Summary
Web 개발을 하다보면, CRUD는 엄청많이 만들어본다. Database에 Table이 신규로 추가되면 엄청 많이 만든다. insert, select, update, delete..  
내가 개발하고 있는 시스템에서 관리하는 자원(데이터, 파일 등)을 Interface로 주고 받는 일은 이전 개발중에도 많이있었고  
그 방법으로,  
DB to DB (A시스템에서 B시스템 Database 서버에 접속하여 데이터를 가져가는 방법)  
EAI (DB to DB 방식으로 했을때의 단점이 너무 많아서, DB 이전 등, 회사의 Database Interface를 관리하는 EAI를 이용하는 방식.)  
API (시스템에서 API로 개발하고, Spec을 전달하여 타 시스템에서 데이터를 가져가는 정도..)  
뭐 더 많을 것이다. NAS에 같은 위치에 파일을 쓴다던지..  

RESTful API의 장점이라고 생각하는건  
  
자원을 classify 하기 쉽다는점.(URI에 directory구조로 들어가는데, 자원의 상위 그룹을 알수 있다.) – 서버단에 폴더 구조도 깔끔해진다.  
  
CRUD에 대한 Method를 어렵게 만들필요가 없다는점. → Node로 개발 해 봤는데, 같은 URI 주소에 대해 GET, POST, PUT, DELETE 로 날리기가 엄청 편하다..  
  
1. 특정 자원이 필요하면, 그 자원을 식별할수 있는 주소를 만든다. e.g., mailbox/u/1/ → mail box에 사용자(user) 1번  
  
2. 해당 자원에 대해 CRUD는 Method를 다르게 만드는것이 아니라.. HTTP Method를 이용하여 만든다.  
  
URI에서 의미하는것이 직관적으로 나타나는게 좋고, 행위가 들어가는게 아니라서 getUserMail ← 이런식의 URL은 없어도 된다는게 좋았던것 같다.