# Docker Network

## 한 줄 요약

Docker network는 container process가 어떤 network 공간에서 실행되고, host나 다른 container와 어떤 주소로 통신하는지를 정하는 Docker 설정이다.

이 문서는 [NetworkBasics.md](./NetworkBasics.md)의 IP 주소, port, endpoint, loopback address 개념을 Docker container 환경에 적용해 설명한다.

## 먼저 알아야 할 역할

- Docker host
  - Docker Engine이 실행되는 Linux host다.
- container process
  - Docker container 안에서 실행되는 application process다.
- container network
  - container process가 보는 network interface, IP 주소, routing table, DNS 설정, port 사용 공간이다.
- Docker network driver
  - Docker가 container network를 어떤 방식으로 만들지 정하는 구현 방식이다.

## Container network란 무엇인가

Docker container는 기본적으로 host와 분리된 network 공간에서 실행된다.

container 안의 process는 자기 network interface, IP 주소, gateway, routing table, DNS 설정을 본다. 이 문서에서는 이 묶음을 `container network`라고 부른다.

container network가 분리되어 있으면 container 안의 `127.0.0.1`은 Docker host가 아니라 해당 container 자신을 가리킨다.

## Bridge network

`bridge network`는 Docker가 container들을 같은 가상 network에 연결하는 방식이다.

같은 bridge network에 연결된 container들은 서로 통신할 수 있다. Docker Compose의 기본 설정에서는 같은 Compose project의 service들이 같은 bridge network에 연결된다.

bridge network에서 container 안의 `127.0.0.1`은 host가 아니라 container 자신이다.

예:

```text
prometheus container
  -> 127.0.0.1:9100
  -> prometheus container 자신
```

따라서 bridge network container가 Docker host의 process에 접근해야 할 때는 `127.0.0.1`만으로는 대상이 맞지 않을 수 있다.

## Port publishing

`port publishing`은 bridge network container의 port를 Docker host의 port로 연결하는 Docker 설정이다.

예:

```yaml
ports:
  - "3000:3000"
```

이 설정은 Docker host의 `3000` port로 들어온 network traffic을 container의 `3000` port로 전달한다.

port publishing은 외부 process가 bridge network container 안의 server process에 접근해야 할 때 사용한다.

## Host network

`host network`는 container가 별도 container network를 쓰지 않고 Docker host의 network 공간을 공유하는 방식이다.

host network container는 별도 container IP를 받지 않는다. container process가 port에 bind하면 Docker host network 공간의 port를 직접 사용한다.

예:

```text
host network container process
  -> bind 127.0.0.1:9100
  -> Docker host의 127.0.0.1:9100에서 접근 가능
```

host network에서는 Docker `ports` mapping을 사용하지 않는다. port가 host network 공간에서 직접 열리기 때문이다.

## Bridge network와 host network 비교

| 항목 | bridge network | host network |
|------|----------------|--------------|
| container network 공간 | Docker host와 분리 | Docker host와 공유 |
| container IP | Docker network 안의 IP를 받음 | 별도 container IP를 받지 않음 |
| container 안의 `127.0.0.1` | container 자신 | Docker host 자신 |
| host port 노출 방식 | `ports`로 publishing | process가 host port에 직접 bind |
| service name DNS | 같은 user-defined bridge network에서 사용 | Compose service name DNS를 사용하지 않음 |

## AutomationProject monitoring 예시

AutomationProject monitoring 설정에서는 node exporter, Prometheus, Grafana가 host network로 실행된다.

node exporter는 다음 주소에 metric endpoint를 연다.

```text
127.0.0.1:9100
```

Prometheus도 host network에서 실행되므로 Prometheus가 보는 `127.0.0.1`은 Docker host의 loopback address다.

그래서 Prometheus는 다음 target을 scrape할 수 있다.

```text
127.0.0.1:9100
```

이 구조에서 `127.0.0.1:9100`은 Prometheus container 내부에만 있는 주소가 아니라 Docker host network 공간의 node exporter endpoint다.

## 자주 헷갈리는 구분

| 표현 | 구분 |
|------|------|
| Docker network와 일반 network | Docker network는 일반 network 개념을 container 실행 환경에 적용한 Docker 설정이다. |
| bridge network와 host network | bridge network는 container network 공간을 분리하고, host network는 Docker host network 공간을 공유한다. |
| host network와 filesystem 공유 | host network는 network 공유 설정이다. host filesystem을 읽는 설정은 volume mount로 따로 정한다. |
| port publishing과 host network | port publishing은 bridge network container port를 host port에 연결하는 설정이고, host network에서는 port publishing을 쓰지 않는다. |
| `127.0.0.1`과 container IP | `127.0.0.1`은 현재 network 공간 자신이고, container IP는 Docker network가 container에 할당한 주소다. |

## 관련 문서

- [NetworkBasics.md](./NetworkBasics.md)
- [HTTPAndBrowser.md](./HTTPAndBrowser.md)
- [WebDeployment.md](./WebDeployment.md)
- [Monitoring.md](./Monitoring.md)
- [Docker networking overview](https://docs.docker.com/engine/network/)
- [Docker bridge network driver](https://docs.docker.com/engine/network/drivers/bridge/)
- [Docker host network driver](https://docs.docker.com/engine/network/drivers/host/)
- [Docker Compose networking](https://docs.docker.com/compose/how-tos/networking/)
