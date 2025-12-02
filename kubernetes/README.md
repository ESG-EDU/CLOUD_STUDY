# Kubernetes 기초 교육

- [쿠버네티스 알아보기](https://github.com/ESG-EDU/Docs/tree/main/kubernetes)

## Kubernetes 구조는 크게 두 영역으로 되어 있습니다.

- `Control Plane`(제어 플레인): 클러스터의 두뇌 역할을 하는 부분입니다. 전체 클러스터의 상태를 관리하고 의사결정을 내립니다.
- `Worker Node`(워커 노드): 실제로 애플리케이션 컨테이너가 실행되는 서버입니다.

### 클러스터 내부의 주요 개념

➤ Pod

- Kubernetes에서 배포 가능한 가장 작은 단위
- 1개 이상의 컨테이너가 하나의 IP를 공유하며 함께 실행

➤ Deployment

- 복수 Pod 관리를 위한 상위 오브젝트
- 애플리케이션의 선언적 관리 (rollout, rollback 제공)

➤ Service

- Pod에 접근하기 위한 네트워크 추상화
	- ClusterIP
	-	NodePort
	- LoadBalancer
	- Headless Service

➤ ConfigMap / Secret

- 애플리케이션 설정 값 관리
- Secret은 Base64 인코딩, 주로 민감 정보 관리

➤ Ingress

- HTTP/HTTPS 라우팅
- 도메인/경로 기반의 외부 트래픽 라우팅

```
          [ Control Plane ]
+---------------------------------------+
|  API Server                           |
|  Scheduler                            |
|  Controller Manager                   |
|  etcd                                 |
+---------------------------------------+

            | (명령, 상태 전달)
            v

          [ Worker Nodes ]
+-----------------------------+
| kubelet   kube-proxy       |
| container runtime           |
| +---------+   +---------+  |
| |  Pod    |   |  Pod    |  |
| | (컨테이너) | | (컨테이너) | |
+-----------------------------+
```

### Manifest(매니페스트) 만들기

- [Nginx POD](./yaml/nginx-pod.yaml)

| 필드 | 설명 |
| :---: | :---: |
| **apiVersion** | Kubernetes API 버전 (`v1`) |
| **kind** | 생성하려는 리소스 종류 (`Pod`) |
| **metadata** | Pod의 이름, 라벨 등 메타데이터 정보 |
| **metadata.name** | nginx-pod | Pod의 고유 이름 |
| **spec** | Pod 내부 구성, 컨테이너 정보 등 동작 정의 |
| **spec.containers[0].name** | Pod에서 실행될 컨테이너 이름 |
| **spec.containers[0].image** | Docker Hub의 nginx 이미지 사용 |
| **spec.containers[0].ports[0].containerPort** | 컨테이너가 사용하는 포트 (Nginx 기본 포트) |

- pod 파일 적용 명령어

```bash
kubectl apply -f nginx-pod.yaml
```

- pod 동작 확인 명령어

```bash
kubectl get pods
```

- pod 내부 접속 명령어

```bash
kubectl exec -it nginx-pod -- bash
```

- pod 실행 시 로컬 포트포워딩

```bash
kubectl port-forward pod/nginx-pod 80:80
```

- pod 삭제 명령어

```bash
kubectl delete pod nginx-pod
```
