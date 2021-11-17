# Pod를 내리고 싶을 때,

1. kubectl로 가능
2. helm chart를 사용해서 helm으로 가능

## 전초작업
### kubectl, helm 설치
kubectl과 helm cli를 설치해야한다.
### context 선택
`kubectl config get-contexts`를 해서 내가 사용할 수 있는 context목록을 살펴본다.
`kubectl config use-context {CONTEXT_NAME}`  
context를 원하는 걸로 선택해야 그 뒤에 사용할 kubectl이 그 context를 기준으로 한다.

## 1. kubectl로 가능
kubectl 설치는 다음에.  
` kubectl delete pod {POD_NAME} -n {NAME_SPACE}`
로 지울 수 있다. `-n {NAME_SPACE}`은 namespace가 있을때 사용. 아니면 필요없다.

## 2. helm chart 사용하는 경우
kubectl로 지워봐야 다시 helm chart를 보고 거기 셋팅이 아직 되어있기 때문에 다시 살아난다. 그러니 helm 설정을 바꾼다.  
`helm list` 해서 내가 선택한 context에 리스트 확인하고,  
`helm delete --purge {NAME}` {NAME}은 리스트에서 봤던 NAME 이다. 이렇게 하면 삭제됨.  
`--purge`를 안하면 완전히 지워지는게 아니라서 새로 배포할 때 꼬일수가 있다. (alb가 올라간 pod를 보지 않고 다른 pod를 본다던지...)
