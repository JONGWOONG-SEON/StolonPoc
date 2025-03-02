### Stolon을 통한 Postgresql HA 구성 관리 예제
- 작성 기준 x86, mac os

### minikube 설치
```
//x86
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
//arm64
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-darwin-arm64
sudo install minikube-darwin-arm64 /usr/local/bin/minikube
```

- 필수요소 다운로드 (minikube kuebectl)

```
minikube kubectl -- get pods -A
minikube addons enable metrics-server
minikube dashboard
```

- pods 배포

```
kubectl apply -f ./yaml
```

- stolon-keeper 레플리카가 실제 데이터베이스 역할을 수행함

```
kubectl run -i -t stolonctl --image=sorintlab/stolon:master-pg10 --restart=Never --rm -- /usr/local/bin/stolonctl --cluster-name=kube-stolon --store-backend=kubernetes --kube-resource-kind=configmap init
```
- 클러스터 configmap 을 초기화 해주어 DBMS 와 클러스터 메타정보 동기화

### 외부에서 접근해보기

- 명령어 `kubectl port-forward svc/stolon-proxy-service 5432:5432` 를 실행하고 localhost:5432 로 접근 
- stolon-secret.yaml 을 수정하지 않았으면 패스워드는 `password1`


### Postgresql 스케일 아웃
- `kubectl scale statefulset stolon-keeper --replicas=5` 를 통해 스케일 아웃 진행

### Postgresql CLI 접근
- `psql --host stolon-proxy-service  --port 5432 postgres -U stolon -W`
