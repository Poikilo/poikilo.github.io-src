---
title: 배포전략(Deployment Strategy)
date: 2020-03-03 18:25:08
tags: [deployment, deploy, CD, continuous delivery, continuous deploy]
---
## 배포 전략
----
### Blue/Green Deployment
Blue/Green 배포 전략은 새 버전의 서버 그룹을 모두 배포 완료한 뒤에,  
로드밸런서에서 트래픽을 구 버전에서 새 버전으로 일시에 바꾸는 방식이다.  
- 장점
  - Downtime 최소화 가능
  - 신규 버전에 문제 발생시 빠른 rollback이 가능하다.
- 단점
  - 인프라 리소스가 2배로 들어간다.
  - 기존 운영되고 있는 서버에서 Long-term 트랜잭션이 수행중이었다면 전환시 어떤 방식으로 처리할 지 충분한 고려가 필요하다.
  - 두 서버 환경간 migration이 배포때 마다 필요하며, Rollback 수행시에도 고려해야 한다.  
![bluegreen_1](https://res.cloudinary.com/practicaldev/image/fetch/s--fJ4tYKdy--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/78dk41w8qmuy9f9pvrf6.png)
![bluegreen_2](https://res.cloudinary.com/practicaldev/image/fetch/s--ca9C-wVZ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/m664yyotixnqncprryf0.png)  

### Rolling Update Deployment
롤링 업데이트 방식은, 새 버전의 서버를 만들어가면서 트래픽을 구 버전 서버에서 신 버전 서버로 점차적으로 옮기는 형태이다.  
구 버전 서버와 신 버전 서버의 비율을 N-K : K (K를 N까지 증가)로 조정하며 점차적으로 부하를 신 버전 서버로 이전하는 방식이다.  
Blue/Green 배포 방식은 인프라 리소스가 2배로 필요한데 비해, 롤링 업데이트 배포는 서버 자원이 한정적인 경우 유리하다.  
- 장점
  - Downtime이 없음.
  - 버전의 업데이트와 테스팅을 동시에 수행할 수 있다.
- 단점
  - 구 버전과 신 버전이 공존하는 시기가 있으므로, 두 버전을 모두 고려하여 개발해야 한다.(e.g., DB schema)
  - 세션 영속성에 대한 고려사항(서버가 롤링 업데이트 상태로 넘어갈 때, 기존 서버에 접속하고 있던 사용자의 session을 신규 서버로 이전해야 한다. 업데이트가 종료된 서버나 업데이트 되지 않은 서버 모두에서 서비스를 이어갈 수 있도록 구성해야 한다.)  
![rollingupdate](https://res.cloudinary.com/practicaldev/image/fetch/s--RbA0NHA6--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/divuxihkun2p186c9mye.png)

### Canary Deployment
> **카나리 테스트**  
> 옛날, 광부들이 광산에서 유독가스가 나오는 것을 알아내기 위해 가스에 민감한 카나리아를 광산안에 키웠다고 한다.
> 카나리아가 죽으면 유독가스가 나오는 것으로 판단하고 조치를 취했다고 하는데, 이 개념을 개발에서 사용하는 것이 카나리 테스트 방식이다.

카나리 배포는 일부서버(10% 정도?)에만 신 버전을 배포하여 운영한 후 문제가 없는것이 확인되면 점차적으로 모든 서버에 신 버전을 배포하는 방식이다.  
- 장점
  - capacity testing을 운영환경에서 진행할 수 있다는 점.(Rollback도 간단하게 가능하다.)
- 단점
  - 동시에 여러개의 소프트웨어 버전을 관리해야 한다는 점.
  - 동시에 2개 이상의 버전이 운영환경에 배포되도록 할 수 있겠지만, 좋은 방법이 아니다..(롤백만 생각해도..)
  - 사용자 PC나 모바일 기기에 설치하는 경우에도 사용이 어렵다.(사용자(고객)를 제한할 수 없다.)  
![canary](https://res.cloudinary.com/practicaldev/image/fetch/s--7PmOiuG9--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/zvf9rbd1x38umph98zro.png)