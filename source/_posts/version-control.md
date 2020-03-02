---
title: 버전관리 도구
date: 2020-03-02 15:03:40
tags: VCS
---
## Configuration Management(CM; 형상관리)
- **Definition**   
CM(형상관리)은 제품의 성능, 기능성, 요구사항 중 물리적 조건, 디자인 제품의 생명주기 전반에 걸친 운영에 필요한 정보 등을 ㅗ구성된 구축과 유지보수에 대한 시스템 엔지니어링 프로세스 이다.  
쉽게 말해서, 제품을 만들고 유지관리하는데 수반되는 프로세스라 생각하면 된다.  
  
- **SCM(Software Configuration Management; 소프트웨어 형상관리)**  
소프트웨어를 개발, 유지보수 하는데 있어 소프트웨어의 변경사항 뿐만 아니라 관련 산출물들에 대한 변경사항을 추적/ 통제하기 위한 프로세스이다.  
  
- **SDLC(Software Development LifeCycle; 소프트웨어 개발 생명주기)**  
소프트웨어의 개발 생명주기. 보통 생각하는 요구사항 분석 - 설계 - 개발 - 테스트 - 이행(배포) - 모니터링 정도로 생각하면 된다.  
SCM은 SDLC의 각 단계별 산출물과, 소스코드의 변경을 추적 관리한다.

- **필요성**  
혼자 개발하는 경우가 거의 없기 때문에.  
개발자(SI)와 운영자(SM)는 다른 경우가 대부분이기 때문에.  
시스템의 규모가 클 수록 변경사항에 대한 체계적 관리가 필요해졌기 때문에.
  
- **개인 의견**  
흔히 형상관리 하면 소스코드의 버전관리를 생각하는 경우가 대부분이다.  
정의에 담겨있듯, 단순히 소스코드의 버전관리가 아니라 SDLC에서 도출되는 모든 산출물이 그 대상이라고 할 수 있다.  
기업마다 관리하는 프로세스의 정도(Level)이 다르기 때문에, 그 범위가 다르다.  
간단한 예로, 자동차에 들어가는 소프트웨어 개발 품질관리 정책과 웹 서비스에 필요한 소프트웨어 개발 프로세스 정책은 확실히 다르다.    
critical한 시스템일 수록 소프트웨어에 대한 품질관리가 철저히 이루어져야 하며 국제 표준또한 많이 나와있다. A-SPICE, ISO-26262 등  

## Tools
- **Git**  
파일의 변경사항을 추적하고, 여러명의 사용자들 간에 해당 파일들의 작업을 조율하기 위한 분산 버전관리 시스템이다.  
대개 소스코드 관리에 사용되지만, 어떤 집합의 파일의 변경사항을 지속적으로 추적하기 위해 사용될 수 있다.(e.g., 산출물 - 기획문서 등 version 관리 가능)  
Git을 기반으로 사용하는 다양한 툴이 개발되어 있다.  
Git의 장점은 브랜치 관리 - [git flow 참고 - Atlassian bitbucket tutorial](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)    
[github](https://github.com)  
[gitlab](https://gitlab.com)  
[bitbucket](https://bitbucket.com)  
- **Subversion**  
Apache 프로젝트, Server-client 모델을 따른 시스템이다.  
서버에 저장소를 두고, 클라이언트에서 push pull 작업을 진행한다.  
Eclipse-java 프로젝트를 진행하며 회사에서 사용 해 보았다. 서버에 설치가 간단하며, CLI, GUI 환경 모두 지원한다.  
windows 계열에는 visual svn(server), tortoiseSVN(client)
Linux 계열에는 subversion 설치 확인.