# Docker Volume and Mount

## 한 줄 요약

Docker mount는 container 안의 특정 filesystem path를 container writable layer가 아니라 Docker volume, host file 또는 host directory 같은 다른 저장 위치와 연결하는 Docker 설정이다.

이 문서는 Docker container가 보는 filesystem과 Docker host의 filesystem이 어떻게 다르고, `volume`, `bind mount`, Compose `volumes:` 설정이 각각 어떤 역할을 하는지 설명한다.

## 먼저 알아야 할 역할

- Docker host
  - Docker Engine이 실행되는 host machine이다.
- container process
  - Docker container 안에서 실행되는 application process다.
- container filesystem
  - container process가 `/`, `/etc`, `/var` 같은 path로 접근하는 filesystem view다.
- container writable layer
  - container가 실행 중 새로 만들거나 수정한 file이 기본으로 저장되는 container별 쓰기 layer다.
- mount source
  - container 안으로 연결할 원본 저장 위치다. Docker volume name, host file path, host directory path가 될 수 있다.
- mount target
  - mount source가 연결되는 container 안의 path다.
- Docker volume
  - Docker가 생성하고 관리하는 persistent storage다.
- bind mount
  - Docker host의 특정 file 또는 directory를 container 안의 path에 직접 연결하는 mount다.

## Container filesystem

container process는 Docker host의 filesystem을 그대로 보는 것이 아니라 container filesystem을 본다.

예를 들어 Grafana process가 container 안에서 `/var/lib/grafana`를 읽으면, 그 path는 Docker host의 `/var/lib/grafana`와 같은 path가 아니다. Docker host의 file을 container process가 읽게 하려면 mount 설정으로 host path 또는 Docker volume을 container path에 연결해야 한다.

Docker는 container 안에서 만들어진 file을 기본적으로 container writable layer에 저장한다. 이 layer는 container별로 따로 존재한다. container를 제거하면 container writable layer의 data도 함께 제거된다. 다른 container나 Docker host process가 그 data를 직접 다루기도 어렵다.

container가 만든 data를 container lifecycle과 분리해서 보관하거나, Docker host에 있는 file을 container가 읽게 하려면 mount를 사용한다.

## Mount

`mount`는 mount source를 mount target에 연결하는 설정이다.

예:

```yaml
volumes:
  - ./config:/app/config:ro
```

이 설정은 Docker host의 `./config` directory를 container 안의 `/app/config` path에 read-only로 연결한다.

각 부분의 의미는 다음과 같다.

| 부분 | 의미 |
|------|------|
| `./config` | mount source. Docker host 쪽 path다. |
| `/app/config` | mount target. container 안의 path다. |
| `ro` | access mode. container process가 이 mount를 읽기 전용으로 사용한다. |

mount가 적용되면 container process가 mount target을 읽을 때 mount source의 내용을 본다.

mount target에 image가 원래 제공하던 file이나 directory가 있더라도, mount가 적용된 동안에는 mount source의 내용이 보인다. 원래 file이 삭제되는 것은 아니지만, container process의 filesystem view에서는 mount 뒤에 가려진다.

## Bind mount

`bind mount`는 Docker host의 file 또는 directory를 container 안의 path에 직접 연결한다.

예:

```yaml
services:
  app:
    volumes:
      - ./app_config.yml:/etc/app/config.yml:ro
```

이 설정에서 `./app_config.yml`은 Docker host의 file이고, `/etc/app/config.yml`은 container 안의 file path다.

bind mount는 다음 기준에 맞을 때 사용한다.

- Git repository에 있는 설정 file을 container가 읽어야 한다.
- 개발 중 source code를 container 안에서 바로 실행해야 한다.
- container가 만든 file을 Docker host의 특정 directory에 남겨야 한다.

bind mount는 Docker host의 directory 구조에 의존한다. 같은 Compose file을 다른 host에서 실행할 때 mount source path가 없으면 container가 기대한 file을 읽지 못할 수 있다.

container가 host file을 수정하면 안 되는 설정 file, dashboard JSON, certificate file은 `:ro` 또는 long syntax의 `read_only: true`를 사용한다.

## Docker volume

`Docker volume`은 Docker가 생성하고 관리하는 persistent storage다.

예:

```yaml
services:
  grafana:
    volumes:
      - grafana_data:/var/lib/grafana

volumes:
  grafana_data:
```

이 설정에서 `grafana_data`는 Docker volume name이고, `/var/lib/grafana`는 container 안의 mount target이다.

Docker volume은 다음 기준에 맞을 때 사용한다.

- database file처럼 container가 실행 중 계속 쓰는 data를 보존해야 한다.
- application 내부 DB, plugin data, runtime state를 container 제거 후에도 남겨야 한다.
- host repository의 특정 file path와 직접 연결할 필요가 없다.

volume data는 container가 제거되어도 volume이 제거되기 전까지 남는다. 따라서 PostgreSQL data directory, Prometheus time series, Grafana 내부 DB처럼 container가 생성하고 계속 갱신하는 runtime data에 적합하다.

## Bind mount와 Docker volume 비교

| 구분 | bind mount | Docker volume |
|------|------------|---------------|
| source 관리 주체 | Docker host directory 구조 또는 repository | Docker |
| container 안 위치 | mount target으로 정한다 | mount target으로 정한다 |
| host에서 직접 file 확인 | source path에서 확인한다 | Docker가 관리하므로 직접 file path에 의존하지 않는다 |
| 적합한 data | source code, 설정 file, dashboard JSON | database data, runtime state, persistent application data |
| host path 의존성 | 있다 | 낮다 |

핵심 기준은 data의 주인이다.

repository가 주인인 file은 bind mount가 맞다. container application이 실행 중 생성하고 보존해야 하는 data는 Docker volume이 맞다.

## Docker Compose volumes syntax

Docker Compose에서 service의 `volumes:`는 container에 mount를 추가하는 설정이다.

짧은 문법은 colon으로 구분한다.

```yaml
volumes:
  - SOURCE:TARGET:ACCESS_MODE
```

각 항목의 의미는 다음과 같다.

| 항목 | 의미 |
|------|------|
| `SOURCE` | Docker volume name 또는 Docker host path |
| `TARGET` | container 안의 mount target |
| `ACCESS_MODE` | `ro`, `rw` 같은 access mode. 생략하면 read-write다. |

`SOURCE`가 `./` 또는 `../`로 시작하면 host path로 읽는다. 이 relative path는 Compose file이 local Docker runtime에서 실행될 때 Compose file이 있는 directory를 기준으로 해석된다.

예:

```yaml
volumes:
  - ./grafana/dashboards:/etc/grafana/dashboards:ro
```

이 설정은 Compose file 위치 기준의 `grafana/dashboards` directory를 container 안의 `/etc/grafana/dashboards`에 read-only로 mount한다.

long syntax를 쓰면 같은 설정을 더 명시적으로 적을 수 있다.

```yaml
volumes:
  - type: bind
    source: ./grafana/dashboards
    target: /etc/grafana/dashboards
    read_only: true
```

## Mount target이 겹칠 때

같은 container filesystem tree 안에 parent path와 child path를 각각 mount할 수 있다.

예:

```yaml
volumes:
  - grafana_data:/var/lib/grafana
  - ./grafana/dashboards:/var/lib/grafana/dashboards:ro
```

이 설정의 의도는 다음과 같다.

| container path | 의도한 source |
|----------------|---------------|
| `/var/lib/grafana` | `grafana_data` Docker volume |
| `/var/lib/grafana/dashboards` | Docker host의 `./grafana/dashboards` directory |

child path mount가 정상 적용되면 `/var/lib/grafana/dashboards`에서는 host directory의 dashboard file이 보인다. 그러나 dashboard bind mount가 누락되거나, Grafana provisioning path와 mount target이 서로 다르면 Grafana process는 repository의 dashboard JSON을 읽지 못한다.

또한 parent path는 runtime data용이고 child path는 repository file용이므로, 같은 tree에 두 책임이 섞인다. 설정을 읽는 사람이 어떤 data가 Docker volume에서 오고 어떤 data가 repository에서 오는지 계속 구분해야 한다.

runtime data와 repository file은 mount target을 분리하면 더 명확하다.

```yaml
volumes:
  - grafana_data:/var/lib/grafana
  - ./grafana/dashboards:/etc/grafana/dashboards:ro
```

이 구조에서는 `/var/lib/grafana`는 Grafana runtime data를 보관하고, `/etc/grafana/dashboards`는 repository에서 관리하는 dashboard JSON을 제공한다. Grafana provisioning file의 dashboard path도 `/etc/grafana/dashboards`를 가리켜야 한다.

## Network와 mount는 다른 설정이다

Docker network는 container process가 어떤 network 공간에서 실행되고 어떤 address와 port로 통신하는지 정한다. 자세한 내용은 [DockerNetwork.md](./DockerNetwork.md)를 참고한다.

Docker mount는 container process가 어떤 filesystem path에서 어떤 file을 볼지 정한다.

따라서 `network_mode: host`를 사용해도 Docker host filesystem이 container 안에 자동으로 공유되지 않는다. host filesystem을 container가 읽어야 하면 별도의 volume mount 또는 bind mount가 필요하다.

## Grafana provisioning 예시

Grafana는 datasource와 dashboard 설정을 file에서 읽는 provisioning 기능을 제공한다. Grafana provisioning 개념은 [Monitoring.md](./Monitoring.md)의 Grafana provisioning 섹션을 참고한다.

Grafana container에서 다음 두 종류의 data는 성격이 다르다.

| data | 적합한 mount |
|------|--------------|
| Grafana 내부 DB, plugin data, session state | Docker volume |
| repository에서 관리하는 dashboard JSON, provisioning YAML | read-only bind mount |

예:

```yaml
services:
  grafana:
    volumes:
      - grafana_data:/var/lib/grafana
      - ./grafana/provisioning:/etc/grafana/provisioning:ro
      - ./grafana/dashboards:/etc/grafana/dashboards:ro

volumes:
  grafana_data:
```

이 설정에서 Grafana process는 runtime data를 `/var/lib/grafana`에 저장하고, provisioning file은 `/etc/grafana/provisioning`에서 읽고, dashboard JSON은 `/etc/grafana/dashboards`에서 읽는다.

Grafana dashboard provisioning YAML의 `path` 값은 dashboard JSON이 mount된 container 안의 path와 같아야 한다.

## 문제를 확인하는 질문

Docker mount 문제를 확인할 때는 다음 질문을 순서대로 확인한다.

1. file을 읽는 process는 Docker host에서 실행되는가, container 안에서 실행되는가?
2. process가 읽는 path는 host path인가, container path인가?
3. Compose `volumes:`의 source는 Docker volume name인가, host path인가?
4. Compose `volumes:`의 target은 application 설정의 path와 같은가?
5. repository file을 읽는 mount에는 `:ro`가 붙어 있는가?
6. runtime data와 repository file이 같은 mount target tree에 섞여 있지는 않은가?

## 자주 헷갈리는 구분

| 표현 | 구분 |
|------|------|
| container filesystem과 host filesystem | container process는 container filesystem을 본다. host file을 보려면 mount가 필요하다. |
| Docker volume과 bind mount | Docker volume은 Docker가 관리하는 persistent storage이고, bind mount는 host path를 container path에 직접 연결한다. |
| mount source와 mount target | source는 연결할 원본 저장 위치이고, target은 container 안에서 보이는 path다. |
| `ro`와 `rw` | `ro`는 read-only이고, `rw`는 read-write다. access mode를 생략하면 read-write다. |
| Docker network와 Docker mount | network는 통신 path를 정하고, mount는 filesystem path를 정한다. |

## 관련 문서

- [DockerNetwork.md](./DockerNetwork.md)
- [Monitoring.md](./Monitoring.md)
- [Docker storage](https://docs.docker.com/engine/storage/)
- [Docker volumes](https://docs.docker.com/engine/storage/volumes/)
- [Docker bind mounts](https://docs.docker.com/engine/storage/bind-mounts/)
- [Docker Compose service volumes](https://docs.docker.com/reference/compose-file/services/#volumes)
