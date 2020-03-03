---
title: node-kubernetes-client
date: 2020-03-03 11:32:37
tags: [kubernetes, node, npm, api]
---
## kubernetes-client 소개
[npm - kuebernetes-client](https://www.npmjs.com/package/kubernetes-client)  
gitlab pipeline의 상태 체크만으로는 kubernetes 클러스터에 온전히 배포가 완료되었는지 체크할 수 없었다.  
kubernetes-client는 Node.js에서 kubernetes api를 사용할 수 있도록 도와주는 패키지이다.  
2020-03-03 기준, kubernetes API 1.13 ~ 1.7 버전까지 지원하고 있다.  

## 간단 사용법
```
const { KubeConfig } = require('kubernetes-client');
const kubeconfig = new KubeConfig();
kubeconfig.loadFormFile('kubeconfig 파일');
const Request = require('kubernetes-client/backends/request');
 
 
const backend = new Request({ kubeconfig });
const client = new Client({backend, version: '1.13'}); // k8s 클러스터의 api version
 
 
kubeconfig.setCurrentContext('context-name'); // 이 기능으로 모든 클러스터에 대한 질의를 할 수 있다.
 
 
// name space 조회
const namesapces = await client.api.v1.namespaces.get()
 
 
// deployment 조회
const deployment = await client.apis.apps.v1.namespaces('<< NameSpace >>').deployments('<< deployment.meta.name >>').get()
```

## 최초의 생각.
kubernetes 클러스터를 여럿 운용하고 있었다.  
Database에 배포 정보를 저장하고, 클러스터 id key를 기록한다.  
1분 크론으로 등록하여, 해당 클러스터 정보에 매핑되는 context-name을 불러온다.  
해당 컨텍스트로 정상 배포되었는지 체크한다.
> deployment의 status가 complate 혹은 success 면 배포 성공이 아닐까?

## 소스코드 작업 진행
```
const { KubeConfig, Client } = require('kubernetes-client');
const kubeconfig = new KubeConfig();
 
 
// 내 PC의 kubeconfig 파일을 선택한다.
kubeconfig.loadFromFile('D:\\kubectl\\kube.conf');
 
const Request = require('kubernetes-client/backends/request');

// Initialize to default context
kubeconfig.setCurrentContext('default');
 
let backend = new Request({ kubeconfig });
let client = new Client({ backend, version: '1.13' });
 
// context를 변경하려면, 아래 setCurrentContext 함수를 통해 변경한다. :: backend와 client를 재생성해야 처리가능하다.
const setCurrentContext = function(context){
  // change to context
  kubeconfig.setCurrentContext(context);
 
  backend = new Request({ kubeconfig });
  client = new Client({ backend, version: '1.13' });
   
  return new Promise(function(resolve, reject){
    resolve(true);
  });
}
 
 
// namespaces 가져오기.
const namespaces = async function(){
  var sNameSpaces = await client.api.v1.namespace.get();
  return new Promise(function(resolve, reject){
    resolve(sNameSpaces);
  });
}
 
// deployments 가져오기.
const deployments = async function(namespace){
  var sDeployments = await client.apis.apps.v1.namespaces(namespace).deployments().get();
 
  return new Promise(function(resolve, reject){
    resolve(sDeployments);
  });
}
 
exports.namespaces = namespaces;
exports.deployments = deployments;
exports.setCurrentContext = setCurrentContext;
```

### namespace.items array elements
```
{ metadata:
   { name: 'default',
     selfLink: '/api/v1/namespaces/default',
     uid: '{UID}',
     resourceVersion: '{resource version -:number}',
     creationTimestamp: '2019-11-15T01:35:51Z' },
  spec: { finalizers: [ 'kubernetes' ] },
  status: { phase: 'Active' } 
}
```

### deployment.items array elements
```
{ metadata:
   { name: 'deployment name',
     namespace: 'namespace name',
     selfLink:
      '/apis/apps/v1/namespaces/default/deployments/deployment_name',
     uid: '{UID}',
     resourceVersion: '{resource_version : number}',
     generation: 81,
     creationTimestamp: '2019-11-21T02:49:31Z',
     labels:
      { app: '{app_name}',
        chart: 'chart_name-1.0.0',
        component: 'app_name',
        heritage: 'Tiller',
        release: 'release_name' },
     annotations: { 'deployment.kubernetes.io/revision': '32' } },
  spec:
   { replicas: 5,
     selector: { matchLabels: [Object] },
     template: { metadata: [Object], spec: [Object] },
     strategy: { type: 'RollingUpdate', rollingUpdate: [Object] },
     revisionHistoryLimit: 5,
     progressDeadlineSeconds: 2147483647 },
  status:
   { observedGeneration: 81,
     replicas: 5,
     updatedReplicas: 5,
     readyReplicas: 5,
     availableReplicas: 5,
     conditions: [ [Object] ] } }
```

### deployment.items[n].status.conditions
```
[ { type: 'Available',
    status: 'True',
    lastUpdateTime: '2019-12-06T03:10:29Z',
    lastTransitionTime: '2019-12-06T03:10:29Z',
    reason: 'MinimumReplicasAvailable',
    message: 'Deployment has minimum availability.' } ]
```

## 문제점
Deployment의 Status는 MinimumReplicasAvailable에서 확인 할 수 있 듯, 최소 replica 수를 만족하는지만 체크한다. :: 배포 정책이 rolling update이기 때문에..

## 차안
Deployment에 연결된 Pod들의 상태를 모두 체크한다.  
Rolling update 배포 방식이어도, 모든 파드가 Running일때만 성공으로 확인하면 배포 상태를 잡을 수 있다.

## 소스 작업 :: 2차
```
const { KubeConfig, Client } = require('kubernetes-client');
const kubeconfig = new KubeConfig();
kubeconfig.loadFromFile('D:\\kubectl\\kube.conf');
const Request = require('kubernetes-client/backends/request');
 
 
// Initialize to default
kubeconfig.setCurrentContext('default');
 
let backend = new Request({ kubeconfig });
let client = new Client({ backend, version: '1.13' });
 
const setCurrentContext = function(context){
  // change to context
  kubeconfig.setCurrentContext(context);
 
  backend = new Request({ kubeconfig });
  client = new Client({ backend, version: '1.13' });
   
  return new Promise(function(resolve, reject){
    resolve(true);
  });
}
 
const namespaces = async function(){
  var sNameSpaces = await client.api.v1.namespace.get();
  return new Promise(function(resolve, reject){
    resolve(sNameSpaces);
  });
}
 
const deployments = async function(namespace){
  var sDeployments = await client.apis.apps.v1.namespaces(namespace).deployments().get();
 
  return new Promise(function(resolve, reject){
    resolve(sDeployments);
  });
}
 
const getDeploymentStatus = async function(namespace, deployment){
  var sDeploymentStatus = await client.apis.apps.v1beta1.namespaces(namespace).deployments(deployment).get();
 
  return new Promise(function(resolve, reject){
    resolve(sDeploymentStatus);
  });
}
 
// Pod Status를 확인하기 위함. api를 활용하여 namespace의 pods.get(QueryString.labelSelector) 체크.
const getPodsStatus = async function(namespace, labelSelector){
  var qs = {
    labelSelector: labelSelector
  }
 
  var sPodStatus = await client.api.v1.namespaces(namespace).pods.get({qs: qs});
 
  return new Promise(function(resolve, reject){
    resolve(sPodStatus);
  })
}
 
exports.namespaces = namespaces;
exports.deployments = deployments;
exports.setCurrentContext = setCurrentContext;
exports.getDeploymentStatus = getDeploymentStatus;
exports.getPodsStatus = getPodsStatus;
```

Pod의 metadata - label을 통해 Deployment에 연결된 Pod들을 조회할 수 있었고
해당 Pod들의 상태가 모두 Running이어야 배포 완료로 체크하여 처리했다.