# Network Basics

## 한 줄 요약

네트워크는 서로 다른 실행 주체가 정해진 주소와 통신 규칙을 사용해 데이터를 주고받는 연결 구조다.

이 문서에서 `실행 주체`는 browser, backend server process, database process, Docker container 안의 process처럼 network로 요청을 보내거나 받을 수 있는 대상을 뜻한다.

## 네트워크란 무엇인가

웹 개발에서 네트워크는 한 process가 다른 process에 request를 보내고 response를 받는 통신 구조를 뜻한다.

예:

```text
browser
  -> HTTP request
  -> backend server
  -> HTTP response
  -> browser
```

같은 컴퓨터 안에서도 network 통신이 일어날 수 있다.

예:

```text
Prometheus
  -> http://127.0.0.1:9100/metrics
  -> node exporter
```

이 예시에서 Prometheus와 node exporter가 같은 Linux host에서 실행되더라도 Prometheus는 HTTP request를 보내고 node exporter는 HTTP response를 돌려준다.

## IP 주소, host, port

`IP 주소`는 network에서 통신 대상을 찾기 위한 주소다.

`host`는 문맥에 따라 network에서 request를 받을 수 있는 컴퓨터, 가상 머신, container 같은 실행 환경을 가리키기도 하고, URL 안에서 그 실행 환경을 찾기 위한 이름이나 주소 부분을 가리키기도 한다.

URL 문맥에서 `host`는 실행 환경 자체가 아니라, request가 도달할 실행 환경을 찾기 위해 URL 안에 적는 이름이나 주소 부분이다. 예를 들어 `http://127.0.0.1:9100/metrics`에서 `127.0.0.1`은 URL의 host 부분이다. `example.com`처럼 domain name이 host 부분에 있으면 client는 DNS 같은 name resolution을 통해 접근할 network 주소를 찾는다.

`port`는 같은 IP 주소 안에서 어떤 server process와 통신할지 구분하는 번호다.

여기서 `server process`는 network request를 받을 수 있게 실행 중인 server 역할의 process다. `server`는 물리 컴퓨터를 뜻하는 말이 아니라 request를 받는 역할을 뜻하고, `process`는 OS에서 실행 중인 프로그램을 뜻한다.

예를 들어 node exporter가 `127.0.0.1:9100`에 bind하고 listen하면, node exporter process가 `9100` port에서 request를 기다리는 server process다. FastAPI backend에서는 FastAPI `app` 객체 자체가 port를 여는 server process가 아니라, 그 앱을 실행하는 Uvicorn 같은 ASGI server process가 network port를 열고 request를 받는다.

예:

```text
127.0.0.1:9100
```

이 값은 `127.0.0.1`이라는 IP 주소의 `9100` port를 뜻한다.

## Protocol과 endpoint

`protocol`은 network 통신에서 request와 response를 어떤 형식과 순서로 주고받을지 정한 규칙이다.

웹 개발에서 자주 보는 `HTTP`는 protocol이다.

`endpoint`는 network로 접근할 대상을 주소, port, path까지 포함해 표현한 값이다.

예:

```text
http://127.0.0.1:9100/metrics
```

이 endpoint는 다음 요소로 나뉜다.

| 요소 | 의미 |
|------|------|
| `http` | protocol |
| `127.0.0.1` | IP 주소 |
| `9100` | port |
| `/metrics` | path |

## 127.0.0.1과 localhost

`127.0.0.1`은 loopback address다. `loopback address`는 network packet을 외부 network로 보내지 않고 현재 network 공간 안으로 되돌리는 주소다.

이 문서에서 `network 공간`은 process가 보는 network 장치, IP 주소, routing table, firewall rule, port 사용 공간을 묶은 실행 환경을 뜻한다.

`localhost`는 local host name resolution에서 loopback address로 해석하도록 쓰는 이름이다. 이 문서에서는 IPv4 loopback address를 명확히 보이도록 `127.0.0.1`을 사용한다.

중요한 점은 `127.0.0.1`이 항상 물리 서버 전체를 뜻하지 않는다는 점이다. `127.0.0.1`은 현재 process가 속한 network 공간 자신을 가리킨다.

| 실행 위치 | `127.0.0.1`의 의미 |
|-----------|--------------------|
| Linux host process | Linux host 자신 |
| Docker bridge network container | 해당 container 자신 |
| Docker host network container | Linux host 자신 |

## Bind와 listen

server process가 특정 IP 주소와 port에서 request를 받도록 여는 동작을 `bind`한다고 표현한다.

예:

```text
127.0.0.1:9100에 bind한다
```

이 말은 현재 network 공간의 `127.0.0.1` 주소와 `9100` port에서 request를 받을 준비를 한다는 뜻이다.

server process가 bind한 주소와 port에서 실제 request를 기다리는 상태를 `listen`한다고 표현한다.

## 같은 컴퓨터 안 통신과 외부 통신

같은 컴퓨터 안의 process끼리도 IP 주소와 port를 사용해 통신할 수 있다.

```text
process A
  -> 127.0.0.1:9100
  -> process B
```

외부 컴퓨터에서 접근해야 하는 server process는 외부에서 도달 가능한 IP 주소나 host 이름에 bind되어 있어야 한다.

```text
browser on user PC
  -> https://example.com
  -> server
```

`127.0.0.1`에만 bind된 server process는 같은 network 공간 안에서는 접근할 수 있지만, 다른 컴퓨터에서 그 주소로 직접 접근할 수 없다.

## AutomationProject monitoring 예시

AutomationProject monitoring 설정에서 node exporter는 `127.0.0.1:9100`에 bind한다.

Prometheus가 같은 network 공간에서 실행되면 다음 endpoint로 node exporter metric을 읽을 수 있다.

```text
http://127.0.0.1:9100/metrics
```

Docker container가 어떤 network 공간에서 실행되는지는 Docker network 설정이 결정한다. Docker에서 이 관계를 이해하려면 [DockerNetwork.md](./DockerNetwork.md)를 함께 읽는다.

## 자주 헷갈리는 구분

| 표현 | 구분 |
|------|------|
| `127.0.0.1`과 public IP | `127.0.0.1`은 현재 network 공간 자신이고, public IP는 외부 network에서 server를 찾는 주소다. |
| IP 주소와 port | IP 주소는 통신 대상을 찾고, port는 그 대상 안의 server process를 구분한다. |
| endpoint와 metric | endpoint는 network로 접근하는 주소이고, metric은 endpoint 응답에 들어 있는 측정값이다. |
| bind와 request | bind는 server process가 받을 주소를 여는 동작이고, request는 client process가 그 주소로 보내는 요청이다. |

## 관련 문서

- [HTTPAndBrowser.md](./HTTPAndBrowser.md)
- [WebDeployment.md](./WebDeployment.md)
- [DockerNetwork.md](./DockerNetwork.md)
- [Monitoring.md](./Monitoring.md)
- [RFC 1122](https://datatracker.ietf.org/doc/html/rfc1122.html)
- [RFC 5735](https://www.ietf.org/rfc/rfc5735)
- [Linux network_namespaces(7)](https://man7.org/linux/man-pages/man7/network_namespaces.7.html)
