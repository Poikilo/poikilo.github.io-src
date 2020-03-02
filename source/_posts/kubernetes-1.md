---
title: kubernetes#1 용어정리
date: 2020-02-27 16:21:00
tags: [kubernetes]
---

#### Kubernetes 용어 정리
|Term|Description|etc|
|----|----|----|
|Cluster|여러 대의 서버(Node)를 묶어, 하나의 자원처럼 사용하는 것.<br><br>||
|Node|클러스터 구성의 기본 단위. 물리서버 1대가 될 수도 있고, Openstack VM Instance 한대가 될 수도 있다.<br><br>||
|Master-Node|Kubernetes의 System Operator 역할, coordinator 역할을 수행하는 Pod들이 올라가 있는 곳.<br>Pod의 Scheduling, Metric 계산(matric-server), Tiller, Dashboard 등이 올라갈 수 있다.<br>kubernetes api 자원관리, 네트워크관리, api 등 kubernetes의 시스템적인 운영을 담당하는 노드이다.<br><br>||
|Worker-Node|사용자가 배포하는 application, service가 동작하는 노드이다.<br><br>||
|CRE|Container Runtime Environment<br>- 컨테이너를 구동하는 환경을 의미한다. <br>- 대표적 예로, Docker와 Container.d가 있다.<br><br>||
|Container|CRE에서 동작하는 단위<br>Virtual Machine과 다르게, Host의 OS, Kernel의 자원을 공유한다.<br>때문에 더 가볍다는 의견.<br><br>||
|Pod|Container를 1개이상 포함하는, Kubernetes의 서비스 최소단위.<br><br>||
|Service|CRE에서 제공하는 내부 네트워크 ip 대역을 추상화 한 개념이다.<br>실제로 사용자는 어떻게 container가 생성되었고, 어떤 내부 ip를 할당받았는지 알수 없기 때문에 kubernetes에서는 Service라는 개념을 도입.<br>Service 명을 통해 Pod에 접속 또는 연결(Connect)하게 된다.<br>__Service는 해당 Pod들에 외부/내부에서 접속하기 위한 IP, Port를 설정__ 할 수 있다.<br>Service는 __Pod와 연결하기 위해 Label Selector__ 라는 개념을 사용한다.<br>Pod의 Metadata에 Label을 기재하도록 하는데, 이를 통해 서비스와 연결한다.<br> (HTML의 Query Selector, JQuery의 Class select 방식을 생각하면 좀 더 이해하기 쉽다.)<br><br>||
|Ingress|ingress는 도메인, path를 통해 어떤 서비스와 연결할지 설정이 가능하다.<br>e.g., domain *.mypage.com 에 대해 backend는 서비스명 homepage-service<br>path는 "/"에 대해 *.mypage.com/ 으로 접속하면 homepage-service와 연결<br>ServicePort는 80 http프로토콜 접속을 허용한다는 것.<br> __외부에서 도메인틀 통한 접속 -> ingress 설정을 확인하여 해당하는 서비스와 연결 -> 해당 서비스에 매핑되어있는 Pod에 연결__ <br><br>||
|Replica Set|Single point failure를 제어하기 위한 개념.<br>Pod의 수를 제어한다.<br>Replica Set, Replica Controller는 Pod의 수를 제어한다. 실제 running중인 Pod의 수가 ReplicaSet에 기재된 replica count보다 작다면, Deployment에 기재된 Pod의 template를 참조하여 Pod를 추가로 생성한다.<br><br>||
|Stateful Set|컨테이너가 제거 또는 재시작 되어도, __상태의 영속성__ 을 보장해야 하는 워크로드에 사용된다.<br>e.g., Database Nginx 등<br><br>||
|Daemon Set|솔루션의 모니터링, 로깅 등을 다른 App의 구동과는 무관하게 지속적으로 실행되어야 하는 app을 위해 사용한다.<br><br>||
|Deployment|Replica Set과 Pod를 정의할 수 있도록 추상화 한 개념<br>Deployment를 kuberctl apply를 통해 적용하면, Deployment, Replica Set, Pod가 생성된다. <br>Deployment를 삭제하더라도, ReplicatSet이 남아있다면 Pod는 삭제되지 않는다.<br>Deployment, ReplicaSet을 모두 제거해야만 Pod가 삭제된다.<br><br>||

<div class="mxgraph" style="max-width:100%;border:1px solid transparent;" data-mxgraph="{&quot;highlight&quot;:&quot;#0000ff&quot;,&quot;nav&quot;:true,&quot;resize&quot;:true,&quot;toolbar&quot;:&quot;zoom layers lightbox&quot;,&quot;edit&quot;:&quot;_blank&quot;,&quot;xml&quot;:&quot;&lt;mxfile host=\&quot;www.draw.io\&quot; modified=\&quot;2020-02-19T03:54:52.703Z\&quot; agent=\&quot;Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.116 Safari/537.36\&quot; etag=\&quot;e54n-G_G8oxnpoMZRh6N\&quot; version=\&quot;12.7.1\&quot; type=\&quot;google\&quot;&gt;&lt;diagram id=\&quot;4Cl5Zr4XpdmF6iXK8EqK\&quot; name=\&quot;Page-1\&quot;&gt;7Vpbc5s4GP01fkwGEJLFY+pkd/PQTmb9kPapI8PHZRZbrpBv++srQGDA6m7acvEkIXlAR+KTpXOO0Hxihhbr45+CbeOPPIB05ljBcYbuZ45ju44zy/+t4FQijkXsEolEEuhWZ2CZ/AsatDS6SwLIWg0l56lMtm3Q55sN+LKFMSH4od0s5Gm71y2L4AJY+iy9RJ+TQMYlSp35Gf8LkiiueraJV9asWdVYjySLWcAPDQg9zNBCcC7Lu/VxAWk+e9W8lM/98YPa+ocJ2MiXPHD3+fHLVxY97j/snIh7z8+bjzc3OsqepTs94JlDUhXvw0rdRPlNBYRc9aOGIU96bsi3Ha8qbrKCuTvVwPa2x3NlFWWR7jIJoo4uuuHfkakQJZmS2wu4oQGnRb0j+G4TQK4sW1Uf4kTCcsv8vPagVgKFxXKd6uowSdMFT7konkW+DzgMFZ5Jwf+BRg0iyENB3d8ehITjD+Vu1yZSyw/wNUhxUk30A0jbTi88Ntblw9nFc09jccPBuPIr0ytHVIc+m0vdaH/9hNecAbw2N3ntk1qGr0NabxkZ21ZQXEPbym3bqn5PN2zlYIOtXDyUrfAAtqImW93DNuWnNRScXoHArlGEYRg6vm8SYUBWBJN+RIjbInRNIkQGEdbq7V2EZKy1/W8lwsRnS5CTUYzzPxPFpLj0SJrrT3H1Qz3pUE8N1NsG6slQzM8HYJ6YmH/iwWSvFjvAMDdR7pE5Yj252mtTi007tjGZpe/M9sSs3dmMYzIxtd5rpDZgWVy37YE1x7oy1qp1ffi37BLEPvFhKlcqT9LANbmSOitEenJl51WK3KlfpfYQ2Sgjv4+bSECWTcYvAxoad8nEp7AKB+HXMdh3ZH7fMyBvCnmdGRDc9dX0KRAbvRvrLSGv01jIuj5juQMYy3g+9nCUIDYs/0V3vv+bexM1smSbFYTFbJuDfsp3wU/zvrIAATHxbgG1KO2Hd9tu835j2Khg01GNPdxOxZRU7k7yJrjLD5iL2WVZlpQbOibkJdyY4jIOBBfHzp3pUn3xnfDhBftl1WcE8n9lfElAc4JNR2EaE5AymezbP9g057qHJ54UVjCfxM27SdhynPqhM3EXcWg7DvE6ccppuIhTCKAe9G9owpTj7UcTcEzkZ3Vv3WJd+lKUaJE1zcv3+RxYVeHUKDyBSNTQQGjspfqaSg02cW9pi0iEvVuP/pomDNGskWVhSgBf3VJBp6Z87jUu3GLMRYp/79f5/4/QiFq3lj2uHkxp43H1MBXP9adOVcKw+ynFS1ntvjIwHdnTrz8/PMk5wIgZZVU8f1pX6uL8hSJ6+A4=&lt;/diagram&gt;&lt;/mxfile&gt;&quot;}"></div>
<script type="text/javascript" src="https://www.draw.io/js/viewer.min.js"></script>