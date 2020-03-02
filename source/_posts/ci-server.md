---
title: CI(Continuous Integration)
date: 2020-03-02 14:16:57
tags: CI S/E
---
## CI(Continuous Integration) : 지속적 통합
- 소프트웨어 공학에서 CI(;지속적 통합)이란, 모든 개발자의 작업 산출물을 하루에 여러번 통합(Merge)하는 작업을 의미한다.
- XP(Extream Programming) 방법론에서는 CI의 컨셉을 적용하여 하루에 수십번 통합하는 작업을 진행한다.

## Common Practice : 일반적 관행
- Maintain a code Repository
 소스 코드 저장소를 유지관리하는 것을 의미한다. : version control.

- Automated Build
 자동 compile -> linking -> build 과정을 거쳐 최종 배포될 산출물을 만드는 것을 의미한다. : build automation

- Make the build self-testing
  코드가 빌드되면, 모든 테스트는 개발자가 의도한대로 동작하는지 테스트를 수행해야 한다. : Unit Test(기능단위 테스트), Integration Test(통합테스트), Static Analysis(정적분석; 코드 품질관리)

- Everyone commits to the base line every day
  정기적인 commit을 하는 것은, 모든 committer가 변경사항의 충돌을 줄일 수 있다. 1주간의 업무점검은 해결하기 어려운 다른 형상과의 충돌위험을 줄일 수 있다.  
  일찍이, 사소한 충돌이 발생하는 것은 팀 멤버들이 자신들이 만드는 것의 변화에 대해 소통하게 한다.  
  적어도 하루에 한번모든 변경사항을 commit 하는 것은 CI의 정의 중 한 부분으로 간주된다.  
  일반적으로, 새벽에 빌드하는 것이 추천된다.

- Every Commit to Baseline shuld be built
  시스템은 커밋된 사항들이 통합된 것(시스템 자체)을 검증하기 위해 현재의 버전에 커밋된 사항을 빌드해야 한다.  
  일반적 관행으로 자동 CI를 사용한다.(몇몇은 수동으로 진행할 수 있다.)  
  CI 자동화 시스템은 지속적인 통합서버(CI Server) 또는 데몬을 사용하여 변경 관리 시스템(revision control system) 을 모니터링 하고, 빌드 프로세스를 자동으로 실행한다.

- Keep the build fast
  빌드는 빠르게 완료될 필요가 있다. 통합에 문제가 발생하면 빠르게 감지되어야 한다.

- Test in a clone of the production environment
  운영 환경과 테스트 환경이 다르기 때문에, 테스트 된 시스템이더라도 운영환경에서 error가 발생할 수 있다.  
  운영환경을 똑같이 복제한 환경을 구축하는 것은 비용이 많이 든다.  
  테스트 환경 대신 pre-production(simulation) 환경을 구축한다. 
  pre-production 환경은 기술스택 구성을 유지하면서 비용을 절감할 수 있다.  
  이러한 테스트 환경에서 서비스 가상화는 의존형에 대한 on-demand access를 얻는데 일반적으로 사용한다.

- Make it easy to get the latest deliverable
  시스템 이해 관계자와 테스터가 쉽게 사용할 수 있는 빌드 도구를 만들면, 요구사항을 충족하지 않는 기능을 다시 빌드할 때 필요한 재 작업의 양을 줄일 수 있다.  
  추가적으로 앞선 테스팅은 배포되기까지 남아있는 결함을 줄일 수 있는 기회를 준다.  
  에러를 빠르게 감지하는 것은 그 에러를 해결하는데 필요한 작업의 양을 줄일 수 있다.

- Everyone can see the result of the latest build
  모든 사람이 최신의 빌드 결과를 확인할 수 있다.  
  이는 빌드의 중단점을 찾기 쉽게 도와주며, 빌드가 중단되었다면 어떤 변화가 중단을 일으켰는지, 중단의 원인이 되는 변경사항을 개발한 사람은 누구인지 확인할 수 있게 한다.

- Automate deployment
  대부분의 CI 시스템은, 빌드가 종료되는 순간 script를 수행할 수 있는 기능을 제공한다.  
  대부분의 경우 운영중인 테스트 서버(모든 사람이 볼 수 있는)에 application을 배포하는 스크립트를 작성한다.  
  이러한 사고방식의 발전으로 지속적인 배포가 등장하게 되고, 지속적인 배포에는 소프트웨어를 운영환경에 직접 배포하고 결함 또는 회귀를 방지하기 위한 추가 자동화가 필요하다.
