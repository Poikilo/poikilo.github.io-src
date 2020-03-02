---
title: JIRA Tip
date: 2020-03-02 17:23:30
tags: jira atlassian issue tracker
---
## 01. 용어 정리
- Projects  
프로젝트는 같은 목적 또는 공동의 맥락을 가진 이슈들의 집합이다.
- Issue  
이슈는 사이즈 무관의 단위 작업을 뜻하며, 시작과 종료까지 Tracking이 필요한 작업을 의미한다.  
다른 말로는 Requests, Tickets, Tasks로 불린다.  
- Workflow  
이슈의 단계는 여러가지가 될 수 있다. 워크 플로우는 이슈 처리에 대한 일련의 작업상태를 뜻한다.  
예시로, Open -> In Progress -> Under Review -> Final Approval -> Done.  
최종 완료상태인 Done/Resolved/Closed를 제외한 각 단계는 서로 ping-pong이 가능하다.  
- Agile  
Agile은 소프트웨어 개발 방법론 중 하나로, 고객의 요청(요구사항)에 따라 빠른 프로토타이핑을 진행하며 Feedback을 통해 소프트웨어를 개선해 나가는 작업을 수행한다.  
지속적인 개발을 통해 서비스를 개선해 나가는 작업에 적합하다. (B2C 서비스들)  
빠른 프로토타이핑을 통해 고객의 의견수렴에 적응이 빠르다.  

## Start with 6 Steps.
**Step 1. Create Project**  
**Step 2. Pick a Template**   
  Template(Scrum, Kanban, Bug Tracking) 제공.  
  팀이나 회사에서 Template을 잘 만들어서 관리하면 산출물 작성에 큰 도움이 된다.  
**Step 3. Board Setup**
  할당된 Issue, 진행중 Issue 등 프로젝트에서 진행하고 있는 Issue에 대한 Dashboard를 제공한다.  
  이를 개인의 목적에 맞게 적용하여 개인 Dashboard에서 활용할 수 있다.  
**Step 4. Issue 생성**  
  Back log에 Issue가 생성된다.  
**Step 5. 팀원 초대**  
  프로젝트에 참여하는 팀원을 초대한다.  
**Step 6. Workflow Setup**  
  Board에서 할당된 Issue의 진행사항등을 Drag & Drop을 통해 상태를 변경한다.  

## JQL
Jira의 데이터를 조회할때 사용하는 Query Language이다.  
잘 사용하면 프로젝트, 이슈의 종류, 일정관리 등에 도움이 된다.  
DB Query(SQL)을 알고 있는 사용자라면, 좀 더 쉽게 배울 수 있다.

- 사용 예
```
// 현재 로그인 한 사용자에게 할당된 이슈 검색.
assignee = currentUser()
```
```
// 내가 담당자인 이슈 중, 처리되지 않은 이슈 검색
assignee = currentUser() and status not in (resolved, closed, done)
```
```
// 특정 이슈의 하위 이슈(sub-task) 검색
Project = {project_name} and parent = {issue_key}
```
```
// 지난주에 상태를 변경한 이슈
status changed during (startOfWeek(-1), endOfWeek(-1)) order by updatedDate
```

## 개인에게 할당된 월별 이슈 조회
개인의 filter를 사용하면 훨씬 더 간단해 지겠지만, JIRA를 건드리지 않고 조회하는 JQL을 설명한다.  
```
// 2020-03에 생성된 이슈이면서, 아직 완료되지 않은 건.
assignee = currentUser() and status not in (closed, resolved, done) and createdDate <= '2020-03-01' and createdDate < '2020-04-01'
```
```
// 2020-03 이전에 생성된 이슈이면서, 아직 완료되지 않은 건.
assignee = currentUser() and status not in (closed, resolved, done) and  createdDate < '2020-03-01'
```
Confluence의 Jira Macro를 활용하여 사용한다면, 매달의 현황을 체크할 수 있다.  

## Issue의 Tag 활용
개발 팀명, 프로젝트 기간, 이슈 타입, 개인화 태그를 추가하면 검색에 좀 더 용이할 수 있다.