---
title: yaml 기본 문법 정리
date: 2020-03-03 11:59:25
tags: [yaml]
---
## 기본 구조
```
parent:
  child-1: first_child
  child-2:
    grandchild-1: first grand child
    grandchild-2: second grand child
```
같은 부모를 갖는 자식 노드들은 들여쓰기가 정확히 일치해야 한다.  
다르면 Error !😂 (**Tab 보다는 space를 활용하자.. vscode의 설정에도 space로 치환하는 것이 있다.**)

## 노드
```
// Type 1
key: value

// Type 2
key:
  - child : 1
    name: tom
  - child : 2
    name: john

// Type 3
key:
  child-1: tom
  child-2: john
```
|TYPE|Description|
|:----:|----|
|Type 1|기본적인 스칼라 형태의 Key Value 선언|
|Type 2|시퀀스로 이루어진 자식노드들, Array 형태|
|Type 3|매핑으로 이루어진 자식노드들(순서를 보장하지 않는다, Key 문자열로 정렬됨)|

## Comments
주석은 # 문자로 시작하는 라인으로만 달 수 있다.  
스칼라 값 옆에 inline 주석은 불가능하다.  
작성할 시, 문자열로 취급된다.
```
# this is valid comment

key: value #this is invalid comment.
```

## anchors/ alias
반복되는 값들은 anchor와 alias를 통해 줄일 수 있다.  
```
anchors:
  # anchor 선언
  first-anchor: &first
    name: tom
    birth: 1999.10.18
  second-anchor: &second
    name: john
    birth: 1995.01.20

first-child: *first
second-child: *second
```
위 코드 처럼 작성하면, first-child key를 통해 first-anchor의 자식노드에 접근 할 수 있다.  
first-child.name  
first-child.birth  
😒😒😒